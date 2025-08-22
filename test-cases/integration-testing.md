## Integration Testing Cases

| Test ID | Objective                          | Modules                      | Input Data                        | Expected Output           | Actual Output   | Status |
|---------|------------------------------------|------------------------------|-----------------------------------|--------------------------|-----------------|--------|
| IT-01   | Login redirects to dashboard       | Auth + Dashboard             | Username: admin<br>Password: Admin@123 | Admin dashboard loads    | Loads           | Pass   |
| IT-02   | Enrollment updates student list    | Student Mgmt + Database      | New student record                | Updated student list      | Updated list    | Pass   |
| IT-03   | Assign course updates faculty list | Course Mgmt + Faculty Mgmt   | Assign CS101 to F001              | Faculty profile updated   | Updated         | Pass   |
| IT-04   | Payment updates balance            | Fee Mgmt + Payment Gateway   | Pay via eSewa                     | Balance updated in system | Updated         | Pass   |  