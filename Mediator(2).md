# Mediator 的实现过程
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/c97b8ea6-dcc0-40fb-a091-5fa7ba66d487)
## 1.在 controller 处引入 mediator,通过依赖注入的方式使用了 IMediator 来处理命令     
🌰这里的例子是 Post ，当需要 get 接口就需要 [HttpGet]
~~~
namespace PractiseForSerena.Api.Controllers;

[ApiController]
[Route("api/[controller]")]
public class PeopleController : ControllerBase
{
    private readonly IMediator _mediator;

    public PeopleController(IMediator mediator)
    {
        _mediator = mediator;
    }

    [HttpPost]
    [Route("create")]
    public async Task<IActionResult> CreateAsync([FromBody] CreatePeopleCommand command)
    {
        var response = await _mediator.SendAsync<CreatePeopleCommand, CreatePeopleResponse>(command).ConfigureAwait(false);

        return Ok(response);
    }
    
    [HttpPost]
    [Route("update")]
    public async Task<IActionResult> UpdateAsync([FromBody] UpdatePeopleCommand command)
    {
        var response = await _mediator.SendAsync<UpdatePeopleCommand, UpdatePeopleResponse>(command).ConfigureAwait(false);

        return Ok(response);
    }
}
~~~
## 2.在这之前 需要先定义命令和响应类，command 和 request（ get 接口用 request ） 
由于 _SendAsync 是 Mediator.Net 包自主分装的函数，所以必须按照给定的方法和类型去使用（ 必须实现继承的接口 ）    
SendAsync 方法将命令（ICommand）或请求（IRequest）发送到处理者（Handler），并返回处理者处理后的响应（IResponse）       
方法规范: Post 请求的两个参数类型是ICommand，IResponse，Get请求的两个参数类型是IRequest，IResponse
~~~
public class CreatePeopleCommand : ICommand
{
    public CreateOrUpdatePeopleDto Person { get; set; }
}

public class CreatePeopleResponse: IResponse
{
    public string Result { get; set; }
}
// 继承接口必须实现该接口
~~~

## 3.在对应的 handler 文件中，去调用对应的 Service 函数执行功能 同时会返回一个 response
在 controller 层引入 mediator 后，Handler 可以帮忙处理 Message
~~~
public class CreatePeopleCommandHandler : ICommandHandler<CreatePeopleCommand, CreatePeopleResponse>
{
    private readonly IPersonService _personService;  // 依赖注入来获取一个 IPersonService 实例，并通过该服务处理命令

    public CreatePeopleCommandHandler(IPersonService personService)
    {
        _personService = personService;
    }

    public async Task<CreatePeopleResponse> Handle(IReceiveContext<CreatePeopleCommand> context, CancellationToken cancellationToken)
    {
      // 从 context.Message 中获取 CreatePeopleCommand 命令，并调用 IPersonService 的 AddPersonAsync 方法执行人员添加操作。
        var @event = await _personService.AddPersonAsync(context.Message, cancellationToken).ConfigureAwait(false);

      // 调用 context.PublishAsync 方法发布添加人员的事件。
        await context.PublishAsync(@event, cancellationToken).ConfigureAwait(false);

        return new CreatePeopleResponse
        {
            Result = @event.Result
        };
    }
}
~~~

## 4.事件Event
⚠️ Get在执行完具体的操作类之后，一般不需要返回Event 事件

PeopleCreatedEvent 用于 接收在 Service层 返回的值 
~~~
namespace PractiseForSerena.Message.Events;

public class PeopleCreatedEvent : IEvent
{
    public string Result { get; set; }
}
~~~
### Event 也会有个对应的 Handler， PeopleCreatedEventHandler

这个🌰简单地返回一个已完成的任务，没有执行任何实际操作

IReceiveContext<PeopleCreatedEvent> context：包含了 PeopleCreatedEvent 事件的上下文信息，可能包括事件的数据和其他相关信息。
~~~
namespace PractiseForSerena.Core.Handler.EventHandler;

public class PeopleCreatedEventHandler : IEventHandler<PeopleCreatedEvent>
{
    public async Task Handle(IReceiveContext<PeopleCreatedEvent> context, CancellationToken cancellationToken)
    {
      // 接收到返回值之后,返回告诉程序已完成任务
        await Task.CompletedTask;
    }
}
~~~
## 5.Service 层
Service 层是用于实现业务逻辑。负责处理应用程序中的业务规则、流程和操作，与数据访问和表示层（如数据库、外部服务、用户界面等）进行交互，从而实现系统功能。
~~~
namespace PractiseForSerena.Core.Services.People;

public class PersonService : IPersonService
{
    private readonly IMapper _mapper;
    private readonly IPersonDataProvider _personDataProvider;

    public PersonService(IMapper mapper, IPersonDataProvider personDataProvider)
    {
        _mapper = mapper;
        _personDataProvider = personDataProvider;
    }

    public async Task<PeopleCreatedEvent> AddPersonAsync(CreatePeopleCommand command, CancellationToken cancellationToken)
    {
       // 对具体的操作类执行之后做一个逻辑处理，也可以在具体操作类执行之前对数据做预处理，可以看 DataProvider 的结果
        var response = await _personDataProvider.CreatePersonAsync(_mapper.Map<Person>(command.Person), cancellationToken).ConfigureAwait(false) > 0 ? "写入成功" : "写入失败";

       // 操作类成功与否都会返回一个结果给 Event,Event也会有一个handler去进行处理，PeopleCreatedEvent 在接收到这个返回值之后，会去执行 PeopleCreatedEventHandler
        return new PeopleCreatedEvent
        {
            Result = response
        };
    }

    public async Task<PeopleUpdatedEvent> UpdatePersonAsync(UpdatePeopleCommand command, CancellationToken cancellationToken)
    {
        var response = await _personDataProvider.UpdatePersonAsync(_mapper.Map<Person>(command.Person), cancellationToken).ConfigureAwait(false) > 0 ? "更改成功" : "更改失败";

        return new PeopleUpdatedEvent
        {
            Result = response
        };
    }
}
~~~

## 6.DataProvider 进行具体的操作，并返回执行结果给 Service 层

~~~
namespace PractiseForSerena.Core.Services.People;

public class PersonDataProvider : IPersonDataProvider
{
    private readonly IRepository _repository;

    private readonly PractiseForSerenaDbContext _unitOfWork;

    public PersonDataProvider(PractiseForSerenaDbContext unitOfWork, IRepository repository)
    {
        _repository = repository;
        _unitOfWork = unitOfWork;
    }

    public async Task<int> CreatePersonAsync(Person person, CancellationToken cancellationToken)
    {
       // InserAsync 是插入一条数据
        await _repository.InsertAsync(person, cancellationToken).ConfigureAwait(false);

       // 保存上面的对数据库操作并返回成功与否的数值（数值>0：成功，数值=0 ：成功但无受影响行数，数值<0：失败）
        return await _unitOfWork.SaveChangesAsync(cancellationToken).ConfigureAwait(false);
    }

    public async Task<int> UpdatePersonAsync(Person person, CancellationToken cancellationToken)
    {
      // UpdateAsync是更新一条数据
        await _repository.UpdateAsync(person, cancellationToken).ConfigureAwait(false);
    
        return await _unitOfWork.SaveChangesAsync(cancellationToken).ConfigureAwait(false);
    }
}
~~~

## 7.要与数据库交互的话，需要提前定义好数据表 Table
在 MySQL 中 varchar 类型 要控制得比较安全要加一下 StringLength（50）
~~~
[Table("people")]
public class Person : IEntity
{
    [Key]
    [Column("id")]
    [DatabaseGenerated(DatabaseGeneratedOption.Identity)]
    public int Id { get; set; }
    
    [Column("first_name"), StringLength(50)]
    public string FirstName { get; set; }
    
    [Column("last_name"), StringLength(50)]
    public string LastName { get; set; }
    
    [Column("creat_time")]
    public DateTimeOffset CreatTime { get; set; } = DateTimeOffset.UtcNow;
}
~~~
