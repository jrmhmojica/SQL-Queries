# SQL Exercises from Hackerrank

## Problem Title: Higher Than 75 Marks
Query the Name of any student in STUDENTS who scored higher than 75 Marks. Order your output by the last three characters of each name. If two or more students both have names ending in the same last three characters (i.e.: Bobby, Robby, etc.), secondary sort them by ascending ID. 

The STUDENTS table is described as follows:

![image](https://user-images.githubusercontent.com/107836788/207787672-372456a6-6260-48af-bc08-d3db69a0df25.png)

Solution:   
`SELECT Name`   
 `FROM STUDENTS`  
 `WHERE Marks > 75`  
 `ORDER BY SUBSTR(Name, -3, 3), ID`
 
 ## Problem Title: Placements
 You are given three tables: Students, Friends and Packages. Students contains two columns: ID and Name. Friends contains two columns: ID and Friend_ID (ID of the ONLY best friend). Packages contains two columns: ID and Salary (offered salary in $ thousands per month).
 
 ![image](https://user-images.githubusercontent.com/107836788/207793434-3d9a9a07-1183-4238-836d-f2fe34a63ff6.png)

Write a query to output the names of those students whose best friends got offered a higher salary than them. Names must be ordered by the salary amount offered to the best friends. It is guaranteed that no two students got same salary offer.

Sample input:

![image](https://user-images.githubusercontent.com/107836788/207793554-3faccbd5-984a-4635-81f0-96e67398392b.png)
![image](https://user-images.githubusercontent.com/107836788/207793572-0a5903b4-6f90-4e8e-a0b9-e148338973cd.png)

Sample output: 

`Samantha`  
`Julia`  
`Scarlet`  

Solution:  
`SELECT s.Name`  
`FROM Students s`  
`JOIN Friends f ON s.ID = f.ID JOIN Packages p ON s.ID = p.ID JOIN Students sf ON sf.ID = f.Friend_ID JOIN Packages fp ON sf.ID = fp.ID`  
`WHERE p.Salary < fp.salary`  
`ORDER BY fp.Salary`


## Problem Title: Interviews
Samantha interviews many candidates from different colleges using coding challenges and contests. Write a query to print the contest_id, hacker_id, name, and the sums of total_submissions, total_accepted_submissions, total_views, and total_unique_views for each contest sorted by contest_id. Exclude the contest from the result if all four sums are 0.
Note: A specific contest can be used to screen candidates at more than one college, but each college only holds one screening contest.

The following tables hold interview data:

- Contests: The contest_id is the id of the contest, hacker_id is the id of the hacker who created the contest, and name is the name of the hacker.  
![image](https://user-images.githubusercontent.com/107836788/207863416-b8e5c965-4460-4ce7-991e-6220f0357e10.png)

- Colleges: The college_id is the id of the college, and contest_id is the id of the contest that Samantha used to screen the candidates.  
![image](https://user-images.githubusercontent.com/107836788/207863514-5e467e01-d787-4c70-ad2b-5a1bbca66e70.png)

- Challenges: The challenge_id is the id of the challenge that belongs to one of the contests whose contest_id Samantha forgot, and college_id is the id of the college where the challenge was given to candidates.  
![image](https://user-images.githubusercontent.com/107836788/207864253-c4f75a0a-e264-4ba5-9a58-56d160eb9606.png)

- View_Stats: The challenge_id is the id of the challenge, total_views is the number of times the challenge was viewed by candidates, and total_unique_views is the number of times the challenge was viewed by unique candidates.  
![image](https://user-images.githubusercontent.com/107836788/207864477-c9b5747d-fcaf-400f-97d0-cccd37329bd0.png)

- Submission_Stats: The challenge_id is the id of the challenge, total_submissions is the number of submissions for the challenge, and total_accepted_submission is the number of submissions that achieved full scores.  
![image](https://user-images.githubusercontent.com/107836788/207864654-da3fbcc8-39ac-46b6-b8a8-cc246a892001.png)


Sample input:
Contests Table:  
![image](https://user-images.githubusercontent.com/107836788/207864988-6735300c-3326-49e3-a0f7-532c30cbf69b.png)  
Colleges Table:  
![image](https://user-images.githubusercontent.com/107836788/207865813-e6cd239e-e3f8-40ad-9c0e-954dd28408f7.png)  
Challenge Table:  
![image](https://user-images.githubusercontent.com/107836788/207865913-70b49f10-9a57-4303-a794-2df8530536db.png)  
View_Stats Table:  
![image](https://user-images.githubusercontent.com/107836788/207866109-0ff99066-c9ed-4b42-99cf-294dc0aa22be.png)  
Submission_Stats Table:  
![image](https://user-images.githubusercontent.com/107836788/207866148-150183ab-b84e-4bdf-bf4a-41b97bf90542.png)  

Sample output:  
`66406 17973 Rose 111 39 156 56`  
`66556 79153 Angela 0 0 11 10`  
`94828 80275 Frank 150 38 41 15`  

Explanation:  
The contest 66406 is used in the college 11219. In this college 11219, challenges 18765 and 47127 are asked, so from the view and submission stats:

Sum of total submissions = 27 + 56 + 28 = 111

Sum of total accepted submissions = 10 + 18 + 11 = 39

Sum of total views = 43 + 72 + 26 + 15 = 156

Sum of total unique views = 10 + 13 + 19 + 14 = 56

Similarly, we can find the sums for contests 66556 and 94828.

Solution:  
`SELECT con.contest_id, con.hacker_id, con.name, SUM(ss.total_submissions), SUM(ss.total_accepted_submissions), SUM(vs.total_views), SUM(vs.total_unique_views)`  
`FROM Contests con`   
`JOIN Colleges col ON con.contest_id = col.contest_id`  
`JOIN Challenges ch ON  col.college_id = ch.college_id`   
`LEFT JOIN`  
`(SELECT challenge_id, SUM(total_views) total_views, SUM(total_unique_views) total_unique_views`  
`FROM View_Stats GROUP BY challenge_id) vs ON ch.challenge_id = vs.challenge_id`  
`LEFT JOIN`  
`(SELECT challenge_id, SUM(total_submissions) total_submissions, SUM(total_accepted_submissions) total_accepted_submissions FROM Submission_Stats GROUP BY challenge_id) ss ON ch.challenge_id = ss.challenge_id`  
`GROUP BY 1,2,3`  
`HAVING SUM(ss.total_submissions)!=0 OR`  
        `SUM(ss.total_accepted_submissions)!=0 OR`  
        `SUM(vs.total_views)!=0 OR`  
        `SUM(vs.total_unique_views)!=0`  
`ORDER BY 1;`  


## Problem Title: Type of Triangle
Write a query identifying the type of each record in the TRIANGLES table using its three side lengths. Output one of the following statements for each record in the table:

Equilateral: It's a triangle with 3 sides of equal length.
Isosceles: It's a triangle with 2 sides of equal length.
Scalene: It's a triangle with 3 sides of differing lengths.
Not A Triangle: The given values of A, B, and C don't form a triangle.

The TRIANGLES table is described as follows:  
![image](https://user-images.githubusercontent.com/107836788/208407208-1b7bb803-973d-475a-b51c-ffe7f75ac48a.png)  
Each row in the table denotes the lengths of each of a triangle's three sides.

Solution:  
`SELECT`  
`  CASE`   
`    WHEN A + B <= C or A + C <= B or B + C <= A THEN 'Not A Triangle'`  
`    WHEN A = B and B = C THEN 'Equilateral'`  
`    WHEN A = B or A = C or B = C THEN 'Isosceles'`  
`    WHEN A <> B and B <> C THEN 'Scalene'`  
`  END`  
`FROM TRIANGLES;`

More queries to come.
