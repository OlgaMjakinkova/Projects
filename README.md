## 🚀 About Me
My name is Olga, and I’m a supply chain manager changing my 
career path to the data analyst field. 
Acquired knowledge and skills in the supply chain 
specialization helped me understand that I’m obsessed
with data. This passion brought me to the Turing college, where
I’m finished learning the data analyst program. 
I’m looking for an opportunity to start my career
in the data world.

## 🛠 Skills
MS Excel, Google Data Studio, dashboards,BigQuery,SQL,MySQL,Data Wrangling,Data cleaning

# Projects

RFM analyse customer segmentation SQL calculation

`code`
WITH t1 as(
          SELECT CustomerID,Country,
          cast(MAX(InvoiceDate) AS date) as Last_PO,
          COUNT(DISTINCT InvoiceNo) as Frequency,
          CASE WHEN ROUND(SUM(UnitPrice*Quantity),2)<0 THEN 0 ELSE ROUND(SUM(UnitPrice*Quantity),2) END as Monetary 
          -- CASE function extracting the negative values ( customer returns or write-offs)  
          FROM tc-da-1.turing_data_analytics.rfm
          WHERE InvoiceDate between '2010-12-01' and '2011-12-01' AND CustomerID IS NOT NULL 
          GROUP BY CustomerID,Country),
      t2 as( 
          SELECT *, 
          DATE_DIFF('2011-12-02', t1.Last_PO, day) as Recency -- date 2011-12-02 was taken as the next day from required data set 2010-12-01 to 2011-12-01.
          FROM t1),
      t3 as(
          SELECT *,
          NTILE(4) OVER (ORDER BY t2.Recency DESC) as R,
          NTILE(4) OVER (ORDER BY t2.Monetary ASC) as M,
          NTILE(4) OVER (ORDER BY t2.Frequency ASC) as F,
          FROM t2),
      t4 as(
          SELECT*,
          CONCAT(R,F,M) as RFM
          FROM t3)

          

          --RMF score calculation
Select *,
CASE WHEN RFM = '444' THEN 'Champions'
     WHEN RFM IN ('334','342','343','344','433','434','443') THEN 'Loyal Customers'
     WHEN RFM IN ('332','333','341','412','413','414','431','432','441','442','421','422','423','424') THEN 'Potential Loyalists'
     WHEN RFM = '411' THEN 'Recent customers' 
     WHEN RFM IN ('311','312','313','331') THEN 'Promising'
     WHEN RFM IN ('212','213','214','231','232','233','241','314','321','322','323','324') THEN 'Customer needing attention'
     WHEN RFM = '211' THEN 'About to sleep'
     WHEN RFM IN ('112','113','114','131','132','133','142','124','123','122','121','223','224','223','222','221') THEN 'At risk'
     WHEN RFM IN ('134','143','144','234','242','243','244') THEN 'Cannot lose them'
     WHEN RFM = '141' THEN 'Hibernating'
     WHEN RFM = '111' THEN 'Lost'
     END as RFM_segment
     FROM t4
 ORDER BY RFM_segment
