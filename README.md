<p>This assignment has a lot of steps. But I'll walk you through them. And after that you will be an Oracle-SQL-Database Ninja.</p>
<p>Do you know what the most common database in business is? Think Excel. It seems every computer has a copy. It seems everybody uses it. Most people use it for the wrong stuff. Despite that it often works. In business, doing what works is more important than doing what's best.</p>
<p>In this exercise, you will be importing the data. Then you will follow a process that known as <strong>Normalizing the Database</strong>. There are a million ways to normalize your database. You're going to import a spreadsheet full of data. Then divide it into different tables. This is a realistic scenario. It plays out everyday throughout corporate America. Clients send you data. The data is a mess. You need that data in a database.</p>
<h4>Here are the steps we will follow</h4>
<ol>
<li>Download the <a id="" class=" instructure_scribd_file instructure_file_link" title="customers.xlsx" href="/courses/1036738/files/42553764/download?wrap=1" target="" data-api-endpoint="https://canvas.instructure.com/api/v1/courses/1036738/files/42553764" data-api-returntype="File">customers.xlsx</a> file. It's an Excel file containing 4,000 rows of data.</li>
<li>Import the customers file into Oracle.</li>
<li>Write a query to find the number of unique companies in the table.</li>
<li>Create a new table for companies and move that data to the new table. Only move the unique companies. Then add a column to your customers table and update the the new column which is called CompanyID.</li>
<li>To set the value of your unique ID you can either create a sequence and a trigger or you can simply update that column to the row number as follows: <code>Update Company set CompanyId = rownum</code>.</li>
<li>Create a new table for cities and a new table for states and positions. Move that data to the new tables in the same way you did above. Move unique cities and unique states to the two new tables.</li>
<li>Delete those columns from this table and add keys to link to the new tables.</li>
<li>Create a query that recreates the list in Excel using joined tables.</li>
<li>Export the data as SQL import statements.</li>
</ol>
<p>Submit your sql from Oracle but you don't have to submit the export file in #9.</p>
<p>&nbsp;</p>
<p>Some help for you......</p>
<div>
<div>you want to move all the unique company names to another table&nbsp;</div>
<div>and then delete them from the customer table.</div>
<div></div>
<pre>--see how many customers are in our table:
select count(*) as "Customer Count" from customers;

--how many unique companies are in our table?
select count(distinct company)as "Distinct Companies" from customers ;

--add a column for the CompanyID to the customers table
alter table customers add CompanyID int;

--notice that the companyId is null
select companyID, company from customers;

--create a table for the companies
--this statement will also create a companyID column which will
--increment when you insert a new record
create table Company (
companyID number generated by default on null as identity,
Company varchar(50)
);

--see what's in your companies table now
select * from Companies;

--add a test company to verify that the identity field works
insert into companies(Company)values ('Dave Company');

--add unique companies from customers to the company table
insert into Companies (Company) select distinct company from customers;

--look at what you've done
select * from Companies;
--another way to select is to list the fields
select companyID, Company from customers;

--update the compnayId in the customers table
update customers c set c.companyID = (select t.companyID from
Companies t where t.company=c.company);

--query to check your data
select c.companyID,c.company,t.companyID,t.Company from
customers c inner join Companies t on c.CompanyID=t.CompanyID;

--remove the company column from the customers table. It is no longer needed.
alter table customers drop column company;

--notice you won't see the company column
select * from customers;

</pre>
</div>
