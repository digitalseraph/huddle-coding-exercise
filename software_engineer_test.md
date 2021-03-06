Basic Rules
===========
1. Review the questions in this file. Reply back via email with an estimated
   delivery date.
2. Answer each of the questions below.
3. Add your answer to each question to this file, in-line.
4. Send the final file back for review.
5. Bonus points if you add this initial file to your public git repo and share
   that repo with us so we can see how your answers progressed.
6. You may use external sources to help answer the questions (i.e. Google, etc),
   but you should always cite your sources in your comments. Learning from
   others is good. Plagiarism is bad.


Developer Test
==============

1. What editor will you use to edit this file, and why?
    - A: I will be using VisualStudio (but I normally use SublimeText)
2. Some of the questions will ask for a solution in the language of
    your choice.  What language(s) will you choose, and why?
    - A: I will be using PHP because it is the one I have the most experience with.
3. Explain the difference between testing and debugging.
    - A: Testing is more preventative and finds issues before an application is pushed to production. The purpose is to make sure the application runs correctly and gives expected outputs and reactions to user interaction.  Debugging is typically done after an issue has already been raised and usually happens after the deployment and/or testing processes have occured. Debugging can be more difficult because, if you tested correctly, the bug would be happening from something unexpected and can take longer to analyze.
4. Consider a user querying a search engine.  Describe, in as much
    detail as you like, what happens between the user clicking the
    "submit" button and the display of the results.
    - A: 
    1. Once a user submits their query, the keywords are sent to the search engine application (through a GET or POST request).
    2. The search engine has already indexed websites, meaning it has made a copy of the site page and related it to the url of that page. This is done by web crawlers. These indexes are typically ordered and sorted by some algorithm. The best known algorithm is called PageRank[ref: 1], which sorts sites based on popularity. Other factors influence this sort and the results, such as how often the site is updated and if it is from a trustworthy site (https, certificates, etc.). These indexes relate keywords to the url as well. The keywords have traditionally been taken from the html meta tags, but some of these process have been changing for awhile.
    3. The user's keywords are compared with this index and a result set is returned.
    4. The user's browser then displays this result set and other filters could possibly be applied such as the user's history and profile. This is usually applied to a Google search, unless the user turns the functionality off in their search settings.
    5. References:
        1. http://www.bbc.co.uk/guides/ztbjq6f

The two tables below describe relationships between employees,
managers, and departments (the columns employee.mgr_id and
department.head both refer to employee.id).  Use these definitions to
answer questions 5-10.  If you need to use any nonstandard functions or
syntax, be sure to name the DBMS that implements them.

employee                              department
----------------------------------    -----------------------
 id |        name        | mgr_id           name      | head
----+--------------------+--------    ----------------+------
  1 | Jonathan Archer    |     11      Operations     |   11
  2 | Christopher Pike   |     12      Marketing      |   12
  3 | James Kirk         |     13      IT             |   13
  4 | Jean-Luc Picard    |     14      HR             |   14
  5 | Kathryn Janeway    |     15      Sales          |   15
  6 | Ralph Wiggum       |     11
  7 | Troy McClure       |     12
  8 | Waylon Smithers    |     17
  9 | Edna Krabappel     |     16
 10 | Ned Flanders       |     15
 11 | Buffy Summers      |
 12 | Xander Harris      |
 13 | Willow Rosenberg   |
 14 | Rupert Giles       |
 15 | Oz Selbie          |
 16 | Dade Murphy        |     11
 17 | Kate Libby         |     13
 18 | Paul Cook          |     17
 19 | Emmanuel Goldstein |     16
 20 | Winston Smith      |     15
 21 | Thomas Anderson    |     15
 22 | Agent Smith        |     14
 23 | Malcolm Reynolds   |     14
 24 | River Tam          |     18
 25 | Jason Nesmith      |     18

5.  Write an SQL query to list the full name of every employee,
    alphabetized by first name.
    ```sql
    SELECT name FROM employee ORDER BY name ASC;
    ```
6.  Write an SQL query to list the full name of every employee,
    alphabetized by last name.
    ```sql
    SELECT left(name, CHARINDEX(' ', name)) AS firstname,
        substring(name, CHARINDEX(' ', name)+1, len(name) - (CHARINDEX(' ', name)-1)) AS lastname
    FROM employee
    ORDER BY lastname ASC;
    ```
7.  Write an SQL query to list the full name of every employee along
    with the full name of his/her manager.
    ```sql
    SELECT t1.name, t2.name
    FROM employee as t1
    JOIN employee AS t2 ON t1.mgr_id = t2.id;
    ```
8.  Write an SQL query to list the full name of every employee in the
    Sales department.
    ```sql
    SELECT name
    FROM employee
    WHERE mgr_id = (SELECT head FROM department WHERE name = 'Sales');
    ```
9.  Write an SQL query to list the full name of every employee along
    with name of his/her department.
    ```sql
    SELECT employee.name, department.name
    FROM employee, department
    JOIN department ON employee.mgr_id = department.head;
    ```
10. Is there a better design for a database that supports the queries
    described in questions 5-9?  If so, describe it.  If not, why not?
    - A: I would eliminate the foreign key fields and just create a linking table called 'employee_department' that links them together, as well as a linking table of 'employee_manager' that links employees to their manager's employee row entry. This can create much faster queries.
11. Write a function in the language of your choice that implements
    quicksort on an array of integers.
    ```php
    public function quicksort($array)
    {
        // PHP implements quicksort in this native function:
        $sorted = sort($array);
        return $sorted;
    }
    ```
12. Write a function in the language of your choice that performs
    binary search on a sorted array of integers.
    ```php
    public function binary_search($array, $key)
    {
        $low = $array[0];
        $high = $array[sizeof($array)];
        $mid = ($high - $low) / 2 + $low;

        while($low <= $high) {
            if($mid < $key) {
                $low = $mid - 1;
            } elseif($mid > $key) {
                $high = $mid + 1;
            } else {
                return true;
            }
        }
        return false;
    }
    ```
13. Write a function in the language of your choice that performs the query
    you wrote for question 7, and outputs the results as an HTML table.
    ```php
    public function employeeWithManager()
    {
        $conn = new mysqli($servername, $username, $password, $dbname);
        if ($conn->connect_error) {
            die("Connection failed: " . $conn->connect_error);
        } 

        $sql = "SELECT t1.name, t2.name as managerName FROM employee as t1 JOIN employee AS t2 ON t1.mgr_id = t2.id";
        $result = $conn->query($sql);

        if ($result->num_rows > 0) {
            // output data of each row
            echo "<table>";
            echo "<tr><th>Employee Name</th><th>Manager Name</th></tr>";
            while($row = $result->fetch_assoc()) {
                echo "<tr>";
                    echo "<td>".$row["name"]."</td>";
                    echo "<td>".$row["managerName"]."</td>";
                echo "</tr>";
            }
            echo "</table>";
        } else {
            echo "0 results";
        }
        $conn->close();
        return true;
    }
    ```
14. Write a program in the language of your choice that takes a filename
    and a number N as arguments and retrieves and outputs the Nth line
    from the file.
    ```php
    public function outputLineFromFile($filename, $line)
    {
        $lineArray = file($filename);
        return $lineArray[$line];
    }
    ```
15. Write the function from question 12 in a different language.
    ```ruby
    def binary_search(arr, key)
        mid = arr[arr.length / 2]
        i = 0
        j = arr.length - 1

        while i < j
            if arr[mid] == key
                return true
            elsif arr[mid] < n
                i = mid + 1
                mid = i + j / 2
            else
                j = mid + 1
                mid = i + j / 2
            end
        end
        return false
    end
    ```
    Referenced this for syntax: https://medium.com/@andrewsouthard1/binary-search-implementation-in-ruby-9636a4bf373c
16. Write the program from question 14 in a different language (it can
    be the same language you used for #15, but it doesn't have to be).
    ```ruby
    def output_line_from_file(filename, line_num)
        File.open(filename) do |file|
            curr_line = 1
            file.each_line do |line|
                return line if line_num == curr_line
                curr_line += 1
            end
        end
    end
    ```
