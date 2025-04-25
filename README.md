# 🧠 Advanced LINQ Practice - Employee & Department Datasets

This repository contains a series of **Advanced LINQ Practice Questions** along with their **answers using method syntax** in C#.  
Datasets used: `Employee` and `Department`.


## 📦 Datasets

### 👨‍💼 Employee & Department

```csharp
public class Employee
{
  public int Id { get; set; }
  public string Name { get; set; }
  public int DepartmentId { get; set; }
  public int Age { get; set; }
  public double Salary { get; set; }
  public string Gender { get; set; }
  public DateTime JoiningDate { get; set; }
}
public class Department
{
  public int Id { get; set; }
  public string DepartmentName { get; set; }
}
```
## ✅ LINQ Questions with Answers

### 1.Get all employees whose salary is greater than 60,000.
    var res = employees.Where(x => x.Salary > 60000).ToList();
### 2.Get the names of all female employees, ordered by age descending.
    var res = employees.Where(x => x.Gender == "Female").OrderByDescending(x => x.Age).Select(x => x.Name).ToList();
### 3.Get the average salary of all employees.
    var res = employees.Average(x => x.Salary);
### 4.Get the employee(s) with the highest salary.
    var res = employees.OrderByDescending(x => x.Salary).FirstOrDefault();
    var maxSalary = employees.Max(x => x.Salary); ###  Another answer
    var res1 = employees.Where(x => x.Salary == maxSalary).ToList();
### 5.Get the count of employees in each department.
    var res = employees.GroupBy(x => x.DepartmentId).Select(g => new
    {
    DepartmentId = g.Key,
    EmployeeCount = g.Count()
    }).ToList();
### 6.Get all employees who are 30 years old or older, and sort them by name.
    var res = employees.Where(x => x.Age >= 30).OrderBy(x => x.Name).ToList();
### 7.Get the total salary paid to employees in the "HR" department.
    var result = employees.Where(x => x.DepartmentId == departments.Where(d => d.DepartmentName == "HR").Select(d => d.Id).FirstOrDefault()).Sum(x => x.Salary);
    result = employees.Join(departments,
    e => e.DepartmentId,
    d => d.Id,
    (e, d) => new { e.Salary, d.DepartmentName })
    .Where(d => d.DepartmentName == "HR")
    .Sum(x => x.Salary);
### 8.Get a list of all employee names along with their department names.
    var res = employees.Join(departments, e => e.DepartmentId, d => d.Id,
    (e, d) => new { e.Name, d.DepartmentName })
    .ToList();
### 9.Get the name(s) of the youngest employee(s).
    var res = employees.Where(x => x.Age == employees.Min(a => a.Age)).ToList();
### 1.Group employees by gender and get the average salary for each gender.
    var res = employees.GroupBy(x => x.Gender).
    Select(x => new
    {
    Gender = x.Key,
    AvgSalary = x.Average(g => g.Salary)
    }).ToList();
### 11.Find the department that has the most employees.
    var res = employees.GroupBy(x => x.DepartmentId)
    .OrderByDescending(g => g.Count())
    .Take(1)
    .Join(departments,
    g => g.Key,
    d => d.Id,
    (g, d) => new
    {
    DepartmentName = d.DepartmentName,
    EmployeeCount = g.Count()
    }).FirstOrDefault();
### 12.Get the list of employees whose name starts with the letter ‘A’.
    var res = employees.Where(e => e.Name.StartsWith("A")).ToList();
### 13.Get the total number of male and female employees.
    var res = employees.GroupBy(e => e.Gender)
    .Select(g => new
    {
    Gender = g.Key,
    EmployeeCount = g.Count()
    }).ToList();
### 14.Find the second highest salary among all employees.
    var res = employees.OrderByDescending(e => e.Salary).Skip(1).FirstOrDefault();
    ### 💡 Bonus tip: If you want just the second highest salary value, you could write:
    var secondHighestSalary = employees
    .Select(e => e.Salary)
    .Distinct()
    .OrderByDescending(s => s)
    .Skip(1)
    .FirstOrDefault();
### 15.Get names of employees whose age is between 25 and 30 (inclusive).
    var res = employees.Where(x => x.Age >= 25 && x.Age <= 30).Select(x => x.Name).ToList();
### 16.Get the names of employees working in the "Finance" department who have a salary greater than 60,000.
    var res = employees.Join(departments,
    e => e.DepartmentId,
    d => d.Id,
    (e, d) => new { e.Name, e.Salary, d.DepartmentName })
    .Where(e => e.DepartmentName == "Finance" && e.Salary > 60000)
    .ToList();
### 17.Get the highest salary in each department.
    var maxSalary = employees.Join(departments, e => e.DepartmentId, d => d.Id,
      (e, d) => new { e.Salary, d.DepartmentName })
      .GroupBy(x => x.DepartmentName)
      .Select(x => new
      {
      DepaermentName = x.Key,
      MaxSalary = x.Max(s => s.Salary)
      }).ToList();
### 18.List all employees along with their department names, even if they don’t belong to any department.
    var res = employees.GroupJoin(departments,
    e => e.DepartmentId,
    d => d.Id,
    (e, deptGroup) => new { e, deptGroup })
    .SelectMany(x => x.deptGroup.DefaultIfEmpty(),
    (x, d) => new
    {
    EmployeeName = x.e.Name,
    DepartmentName = d != null ? d.DepartmentName : "No Department"
    }).ToList();
### 19.Get all employees who joined in the year 2023.
    var res = employees.Where(x => x.JoiningDate.Year == 2023).ToList();
### 20.Get the number of employees who have not been assigned to any department.
    var res = employees.Where(x => x.DepartmentId == null).Count();
### 21.List names of employees who are the only ones in their department.
    var res = employees.GroupBy(e => e.DepartmentId)
    .Where(g => g.Count() == 1)
    .SelectMany(g => g)
    .Select(x => x.Name)
    .ToList();
### 22.Find the average age of employees in each department.
    var res = employees
    .Join(departments, e => e.DepartmentId, d => d.Id,
    (e, d) => new { e.Age, d.DepartmentName })
    .GroupBy(e => e.DepartmentName)
    .Select(x => new
    {
    DepartmentName = x.Key,
    AvarageAge = x.Average(g => g.Age)
    })
    .ToList();
### 23.Get names of employees who have the same salary.
    var res = employees.GroupBy(x => x.Salary)
    .Where(x => x.Count() > 1)
    .SelectMany(g => g)
    .Select(x => x.Name)
    .ToList();
### 24.Get the most recently joined employee from each department.
    var res = employees.Join(departments,
    e => e.DepartmentId,
    d => d.Id,
    (e, d) => new { e.JoiningDate, d.DepartmentName })
    .GroupBy(g => g.DepartmentName)
    .Select(g => g
    .OrderByDescending(x => x.JoiningDate)
    .FirstOrDefault())
    .ToList();
### 25.Get a list of departments along with the number of employees in each, including departments with zero
    var res = departments.GroupJoin(employees,
       e => e.Id,
       d => d.DepartmentId,
       (e, deptGroup) => new
       {
       DepartmentName = e.DepartmentName,
       EmployeeCount = deptGroup.Count()
       })
       .ToList();
### 26.Get the name(s) of department(s) with no employees assigned.
    var res = departments.GroupJoin(employees,
    d => d.Id,
    e => e.DepartmentId,
    (e, deptGroup) => new
    {
    DepartmentName = e.DepartmentName,
    EmployeeCount = deptGroup.Count()
    })
    .Where(x => x.EmployeeCount == 0)
    .ToList();
### 27.Get top 3 highest-paid employees.
    var res = employees.OrderByDescending(e => e.Salary)
    .Take(3)
    .ToList();
### 28.List departments along with the total salary paid to their employees.
    var res = employees.Join(departments,
    e => e.DepartmentId,
    d => d.Id,
    (e, d) => new { e.Salary, d.DepartmentName })
    .GroupBy(d => d.DepartmentName)
    .Select(x => new
    {
    DepartmentName = x.Key,
    TotalSalary = x.Sum(e => e.Salary)
    })
    .ToList();
### 29.Find the employee(s) with the second lowest salary.
    var secondLowestSalary = employees
    .Select(e => e.Salary)
    .Distinct()
    .OrderBy(s => s)
    .Skip(1)
    .FirstOrDefault();
    var res = employees
    .Where(e => e.Salary == secondLowestSalary)
    .ToList();
### 30.List departments where average employee age is more than 30.
    var res = employees.Join(departments,
    e => e.DepartmentId,
    d => d.Id,
    (e, d) => new { e.Age, d.DepartmentName })
    .GroupBy(e => e.DepartmentName)
    .Select(g => new
    {
    DepartmentName = g.Key,
    Age = g.Average(x => x.Age)
    })
    .Where(g => g.Age > 30)
    .ToList();
### 31.List employee names grouped by department, where each group includes total salary of that department.
    var res = employees.Join(departments,
    e => e.DepartmentId,
    d => d.Id,
    (e, d) => new { e.Name, e.Salary, d.DepartmentName })
    .GroupBy(x => x.DepartmentName)
    .Select(g => new
    {
    Names = g.Select(x => x.Name).ToList(),
    DepartmentName = g.Key,
    TotalSalary = g.Sum(x => x.Salary)
    })
    .ToList();
### 32.List employees who joined in the last 6 months.
    var res = employees.Where(x => x.JoiningDate > DateTime.Now.AddMonths(-6))
    .ToList();
### 33.Find the department with the highest average salary.
    var res = employees.Join(departments,
    e => e.DepartmentId,
    d => d.Id,
    (e, d) => new { e.Salary, d.DepartmentName })
    .GroupBy(d => d.DepartmentName)
    .Select(g => new
    {
    DepartmentName = g.Key,
    AvgSalary = g.Average(x => x.Salary)
    })
    .OrderByDescending(x => x.AvgSalary)
    .FirstOrDefault();
### 34.List all employees along with their department name. If department is not assigned, show 'No Department'.
    var res = employees.GroupJoin(departments,
    e => e.DepartmentId,
    d => d.Id,
    (e, deptGroup) => new { e, deptGroup })
    .SelectMany(x => x.deptGroup.DefaultIfEmpty(),
    (e, d) => new
    {
    EmployeeName = e.e.Name,
    DepartmentName = d != null ? d.DepartmentName : "No Department"
    })
    .ToList();
### 35.Find the most recent joining employee in each department.
    var res = employees.Join(departments,
    e => e.DepartmentId,
    d => d.Id,
    (e, d) => new { e.Name, e.JoiningDate, d.DepartmentName })
    .GroupBy(g => g.DepartmentName)
    .Select(g =>
    {
    var letest = g.OrderByDescending(x => x.JoiningDate).First();
    return new
    {
    EmployeeName = letest.Name,
    DepartmentName = letest.DepartmentName,
    JoiningDate = letest.JoiningDate
    };
    })
    .ToList();
### 36.Find the employee(s) who joined earliest overall.
    var res = employees.OrderBy(x => x.JoiningDate)
    .FirstOrDefault();
### 37.Find all employees who have the same joining date.
    var res = employees.GroupBy(x => x.JoiningDate)
    .Where(g => g.Count() > 1)
    .SelectMany(g => g)
    .Select(x => x.Name)
    .ToList();
### 38.List departments with no employees.
    var res = departments.GroupJoin(employees,
    d => d.Id,
    e => e.DepartmentId,
    (d, e) => new { d, e })
    .SelectMany(g => g.e.DefaultIfEmpty(),
    (d, e) => new
    {
    DepartmentName = d.d.DepartmentName,
    EmployeeName = e?.Name
    })
    .Where(x => x.EmployeeName == null)
    .ToList();
### 39.Find employees whose name contains 'an' (case-insensitive)
    var res = employees.Where(x => x.Name.ToLower().Contains("an")).ToList();
    var res = employees.Where(x => x.Name.Substring(0, 1) == "G").ToList();
### 40.List top 2 youngest employees in each department.
    var res = employees.GroupBy(x => x.DepartmentId)
    .SelectMany(g => g.OrderBy(x => x.Age))
    .Take(2)
    .ToList();
### 41.Find employees who earn less than average salary of their department.
    var res = employees.Join(departments,
    e => e.DepartmentId,
    d => d.Id,
    (e, d) => new { e.Name, e.Salary, d.DepartmentName })
    .GroupBy(g => g.DepartmentName)
       .SelectMany(g =>
       {
     var avgSalary = g.Average(x => x.Salary);
     return g.Where(x => x.Salary < avgSalary)
     .Select(x => new
     {
       x.Name,
       x.Salary,
       DepartmentName = x.DepartmentName,
       AvgSalary = avgSalary
     });
       })
       .ToList();
### 42.List all departments along with the count of employees who joined in the last 1 year.
    var res = employees.Join(departments,
    e => e.DepartmentId,
    d => d.Id,
    (e, d) => new { e.JoiningDate, d.DepartmentName })
    .Where(e => e.JoiningDate >= DateTime.Now.AddYears(-1))
    .GroupBy(g => g.DepartmentName)
    .Select(g => new
    {
    DepartmentName = g.Key,
    JoinningCount = g.Count()
    })
    .ToList();
### 43.Find employees whose department has more than 2 employees.
    var res = employees
       .GroupBy(e => e.DepartmentId)
       .Where(g => g.Count() > 2)
       .SelectMany(g => g)
       .Join(departments,
       e => e.DepartmentId,
       d => d.Id,
       (e, d) => new
       {
       e.Name,
       d.DepartmentName
       })
       .ToList();
### 45.Find the department which has the maximum number of employees.
    var res = employees.GroupBy(e => e.DepartmentId)
    .OrderByDescending(g => g.Count())
    .Take(1)
    .Join(departments,
    g => g.Key,
    e => e.Id,
    (e, d) => new
    {
    Departmentname = d.DepartmentName,
    EmployeeCount = e.Count()
    })
    .FirstOrDefault();
### 46.Find departments where average employee age is less than 30.
    var res = employees
    .Join(departments,
    e => e.DepartmentId,
    d => d.Id,
    (e, d) => new { e.Age, d.DepartmentName })
    .GroupBy(x => x.DepartmentName)
    .Where(g => g.Average(x => x.Age) < 30)
    .Select(g => new
    {
    DepartmentName = g.Key,
    AvgAge = g.Average(x => x.Age)
    })
    .ToList();
### 47.Find all employees whose name is repeated (i.e., duplicate names)
    employees.GroupBy(x => x.Name)
                .Where(g => g.Count() > 1)
                .ToList();
### 48.Find the employee count department-wise and gender-wise.
    employees.GroupBy(x => new { x.DepartmentId, x.Gender })
                .Join(departments,
                x => x.Key.DepartmentId,
                d => d.Id,
                (e, d) => new
                {
                    DepartmentName = d.DepartmentName,
                    Gender = e.Key.Gender,
                    EmployeeCount = e.Count()
                })
                .ToList();
### 49.Find the employee(s) who joined on the same date (i.e., duplicate joining dates)
    employees.GroupBy(x => x.JoiningDate)
                .Where(g => g.Count() > 1)
                .SelectMany(g => g)
                .ToList();
### 50.Find the department(s) that have more than 2 employees.
    employees.GroupBy(x => x.DepartmentId)
                .Where(e => e.Count() > 2)
                .Join(departments,
                e => e.Key,
                d => d.Id,
                (e, d) => new
                {
                    DepartmentName = d.DepartmentName
                })
                .ToList();
