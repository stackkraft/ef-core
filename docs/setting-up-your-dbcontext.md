# Setting up your DbContext
To interact with your database from within your .NET code, you will need to create a class which inherits the 
`DbContext` class.

```csharp
public class BloggingDbContext : DbContext
{
}
```

The `DbContext` class is part of NuGet package `Microsoft.EntityFrameworkCore`, so if the class `DbContext` is not yet 
recognized in your code, you will need to add this NuGet package.

You should be able to compile your project now, although it will not do anything yet. 
This is because we haven't added any entity classes yet and we still need to setup the connection with our database.

## Adding your entity classes

Your database needs one or more tables to store the data in from your application. 
Each of these tables is represented by a so called _entity_ class, which is a representation of a database table in code.

An entity class consists of properties, and these properties represent the columns and relations in your database table.

In the example below there are two entity classes: `Blog` and `Post`. 
These entities will be mapped onto two database tables: `Blogs` and `Posts`.

```csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; } = new();
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public int BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

To add your entity classes to your DbContext class, you will to define properties for each of the entity classes using
the `DbSet` type. That will look like the example below.

```csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```

## Configure your database

The last step is to configure your database so EF Core knows how to interact with it. 
This step is specific for different database systems, so you will need check how this is done for your specific 
database.

In the example below a connection to a SQL Server database is being configured.

```csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlServer("Server=(local);Database=MyDatabase;Trusted_Connection=True;TrustServerCertificate=True;");
    }
}
````

The `UseSqlServer` method is specific for the SQL Server provider in EF Core and requires a specific NuGet package 
`Microsoft.EntityFrameworkCore.SqlServer`. You need to add this NuGet package to your project to be able to configure
the connection to your SQL Server database.

Now you have setup your DbContext with entities and a connection to your database, it is time to create your first 
migration. This initial migration will create the tables in your database according to the entity models that were 
created before. Once these tables are created, your application can interact with your database.

See [Creating your first migration](creating-your-first-migration.md)

  

