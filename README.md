# employee-salary
-- Department Table
CREATE TABLE Department (
    DeptID INT PRIMARY KEY,
    DeptName VARCHAR(100)
);

-- Employee Table
CREATE TABLE Employee (
    EmpID INT PRIMARY KEY,
    EmpName VARCHAR(100),
    DeptID INT,
    Designation VARCHAR(100),
    FOREIGN KEY (DeptID) REFERENCES Department(DeptID)
);

-- Salary Table
CREATE TABLE Salary (
    EmpID INT PRIMARY KEY,
    BasicPay DECIMAL(10,2),
    FOREIGN KEY (EmpID) REFERENCES Employee(EmpID)
);

-- Allowances Table
CREATE TABLE Allowances (
    EmpID INT PRIMARY KEY,
    HRA DECIMAL(10,2),
    TA DECIMAL(10,2),
    FOREIGN KEY (EmpID) REFERENCES Employee(EmpID)
);

-- Deductions Table
CREATE TABLE Deductions (
    EmpID INT PRIMARY KEY,
    Tax DECIMAL(10,2),
    PF DECIMAL(10,2),
    FOREIGN KEY (EmpID) REFERENCES Employee(EmpID)
);
-- Department
INSERT INTO Department VALUES (1, 'HR'), (2, 'IT'), (3, 'Finance');

-- Employee
INSERT INTO Employee VALUES (101, 'Amit Sharma', 2, 'Software Engineer');

-- Salary
INSERT INTO Salary VALUES (101, 50000);

-- Allowances
INSERT INTO Allowances VALUES (101, 10000, 5000);

-- Deductions
INSERT INTO Deductions VALUES (101, 5000, 2000);
SELECT 
    e.EmpID,
    e.EmpName,
    e.Designation,
    d.DeptName,
    s.BasicPay,
    a.HRA,
    a.TA,
    s.BasicPay + a.HRA + a.TA AS GrossSalary,
    deduct.Tax,
    deduct.PF,
    (s.BasicPay + a.HRA + a.TA - deduct.Tax - deduct.PF) AS NetSalary
FROM 
    Employee e
JOIN Department d ON e.DeptID = d.DeptID
JOIN Salary s ON e.EmpID = s.EmpID
JOIN Allowances a ON e.EmpID = a.EmpID
JOIN Deductions deduct ON e.EmpID = deduct.EmpID
WHERE e.EmpID = 101;
