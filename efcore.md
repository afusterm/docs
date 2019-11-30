# EF Core

## To use EF Core

- Install Microsoft.EntityFramework.Core.Sqlite and Microsoft.EntityFrameworkCore.Design:

`dotnet add package Microsoft.EntityFrameworkCore.Sqlite`

`dotnet add package Microsoft.EntityFrameworkCore.Sqlite`

- Run `dotnet restore` to install new packages.

## Changing the model

- If you make changes to the model: `dotnet ef migrations add <migration name>`

Ex: `dotnet ef migrations add InitialCreate`

- Update the database

`dotnet ef database update`