# awesome_chocolate_dataset
A dataset about a chocolate company. 

The data set contains 4 tables each with information and observations about Sales, Products, People and Geo about Awesome Chocolate company

Let's try to answer some business questions

```
/* details of shipments (sales) where amounts are > 2,000 and boxes are <100?*/
select * from sales
where amount > 2000 and boxes <1000;

/*How many shipments (sales) each of the sales persons had in the month of January 2022?*/
select distinct p.salesperson, count(*) number_of_sales
from people p 
join sales s on s.spid = p.spid
where s.SaleDate between '2022-1-1' and '2022-1-31'
group by 1;
/*obs out of 33 salesperson, 25 made sales in january 2022*/

/* Which product sells more boxes? Milk Bars or Eclairs?*/
select pr.product, sum(boxes) as 'Total Boxes'
from sales s
join products pr on s.pid = pr.pid
where pr.Product in ('Milk Bars', 'Eclairs')
group by pr.product;
/*obs: Eclairs sell more boxes*/

/*Which salespersons did not make any shipments in the first 7 days of January 2022?*/
select p.salesperson
from people p
where p.spid not in
(select distinct s.spid from sales s where s.SaleDate between '2022-01-01' and '2022-01-07');
/*obs we confirm and get the names of salesperson who never made sales in january 2022*/

/*How many times we shipped more than 1,000 boxes in each month*/
select year(saledate) Year, month(saledate) Month, count(*) 'Times we shipped 1k boxes'
from sales
where boxes>1000
group by year(saledate), month(saledate)
order by year(saledate), month(saledate);
/*we can order by count to get times we shipped 1k boxes in asecending order*/

/* India or Australia? Who buys more chocolate boxes on a monthly basis?*/
select year(saledate) Year, month(saledate) Month,
sum(CASE WHEN g.geo='India'  THEN boxes ELSE 0 END) 'India Boxes',
sum(CASE WHEN g.geo='Australia' THEN boxes ELSE 0 END) 'Australia Boxes'
from sales s
join geo g on g.GeoID=s.GeoID
group by year(saledate), month(saledate)
order by year(saledate), month(saledate);```
