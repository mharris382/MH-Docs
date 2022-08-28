[Getting Started](https://docs.microsoft.com/en-us/dotnet/core/testing/unit-testing-with-nunit)
==


Windows ([[PowerShell]])
--
How to use [[NUnit]] in a standard .Net class library which doesn't rely on any external engine libraries like Unity or Godot

1. open powershell and navigate to directory where you want to place the solution
2. use command `dotnet new sln`
3. create a subfolder for the Module you want to implement (Example: `Module.Core`)
4. navigate to subfolder `cd Module.Core`
5. use command `dotnet new classlib`
6. rename class1.cs 
7. return to root directory and create another subfolder for the tests (Example `Module.Core.Tests`)
8. naviagte to the testing subfolder `cd Module.Core.Tests
9. use command `dotnet new nunit
10. use command `dotnet add reference ../Module.Core/Module.Core.csproj`
11. return to the parent and use command `dotnet sln add ./Module.Core.Tests/Module.Core.Tests.csproj`
12. now that everything is setup, write the test code so that it fails
13. use command `dotnet test` to execute your tests to confirm the test fails
15. implement the tested feature and then run command `dotnet test` to rerun the tests and confirm they now pass


