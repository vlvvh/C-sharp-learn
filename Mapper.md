# 映射 Mapper
映射（Mapping）在软件开发中通常是指将一个对象的属性和数据转换成另一个对象的属性和数据的过程。
可以帮助将数据从一个层次或形式转换到另一个层次或形式，例如从数据库实体到数据传输对象（DTO）、从表单数据到业务对象等。
## 1️⃣ 手动映射
直接编写代码来将一个对象的属性复制到另一个对象。
~~~
public class Source
{
    public int Id { get; set; }
    public string Name { get; set; }
}

public class Destination
{
    public int Id { get; set; }
    public string Name { get; set; }
}

public Destination MapToDestination(Source source)
{
    return new Destination
    {
        Id = source.Id,
        Name = source.Name
    };
}
~~~
✅ 优点：
- 完全控制
- 灵活性高

❌ 缺点：
- 重复代码多
- 维护成本高

## 2️⃣ AutoMapper 使用"Map"方法
AutoMapper 提供了 Map 方法用于在内存中进行对象之间的映射。
~~~
var entity = await _repository.GetEntityByIdAsync(id);
var dto = _mapper.Map<EntityDto>(entity);
~~~
✅ 优点：
- 不需要手动编写所有映射的代码
- 配置集中管理，便于维护

❌ 缺点：
- 在复杂映射时，需要详细的配置

## 3️⃣ 构造函数映射
在目标对象的构造函数中接受源对象，并在构造函数内进行数据的复制。
~~~
public class Destination
{
   public int Id { get; set;}
   public string Name { get; set;}

   public Destination (Source source)
   {
       Id = source.Id;
       Name = source.Name;
   }
}
~~~
✅ 优点：
- 在初始化时就可以完成映射
- 可封装初始化逻辑

❌ 缺点：
- 增加类之间的耦合
- 依赖构造函数参数

## 4️⃣ LINQ Select 映射
在目标对象的构造函数中接受源对象，并在构造函数内进行数据的复制。适用于从数据库直接投影到 DTO 的情况，类似于 ProjectTo 但没有 AutoMapper 的额外依赖。
~~~
var dtoList = await _repository.Query<Entity>()
    .Where(e => e.IsActive)
    .Select(e => new EntityDto
    {
        Id = e.Id,
        Name = e.Name,
        Description = e.Description
        // 其他属性映射
    })
    .ToListAsync();
~~~
✅ 优点：
- 可以直接在查询中进行映射
- 类型安全，简洁

❌ 缺点：
- 适用于简单映射
- 复杂映射逻辑不易实现

## 5️⃣ AutoMapper 使用"ProjectTo"方法
用于将数据查询结果直接映射到目标类型（DTO），从而简化映射代码并提高性能。
~~~
public async Task<List<UserDto>> GetUsersAsync(CancellationToken cancellationToken)
    {
        var userDtos = await _context.Set<User>()
            .ProjectTo<UserDto>(_mapper.ConfigurationProvider)
            .ToListAsync(cancellationToken);

        return userDtos;
    }
~~~
✅ 优点：
- 查询阶段就可以完成映射
- 支持复杂映射
- 简化代码

❌ 缺点：
- 配置复杂性
- 生成复杂的SQL查询开销可能会影响性能
- 依赖第三方库"AutoMapper"
