## Unit Testing Cases

| Test ID | Objective                         | Module         | Input Data                              | Expected Output           | Actual Output           | Status |
|---------|-----------------------------------|----------------|-----------------------------------------|--------------------------|-------------------------|--------|
| UT-01   | Login with valid admin credentials| Authentication | Username: admin<br>Password: Admin@123  | Dashboard loads           | Dashboard loads         | Pass   |
| UT-02   | Login with invalid password       | Authentication | Username: admin<br>Password: wrongpass  | Error: Invalid credentials| Error displayed         | Pass   |
| UT-03   | Enroll a new student              | Student Mgmt   | Name: Sita Lama<br>Roll: 2301           | Student added             | Student added           | Pass   |
| UT-04   | Create new course                 | Course Mgmt    | Course: CS101                           | Course added              | Course added            | Pass   |
| UT-05   | Generate invoice                  | Fee Mgmt       | Roll: 2301                              | PDF invoice generated     | PDF generated           | Pass   |