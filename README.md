# SQL_Skills_Assessment

Here is the SQL query to pull the most recent redemption count by redemption date for the retailer "ABC Store" within the specified date range.

WITH MostRecentRedemptions AS (
  SELECT
    r.retailerName,
    rb.redemptionDate,
    rb.redemptionCount,
    rb.createDateTime,
    ROW_NUMBER() OVER (PARTITION BY rb.redemptionDate ORDER BY rb.createDateTime DESC) AS rn
  FROM
    `tblRedemptions-ByDay` rb
  JOIN
    `tblRetailers` r
  ON
    rb.retailerId = r.id
  WHERE
    r.retailerName = "ABC Store"
    AND rb.redemptionDate BETWEEN '2023-10-30' AND '2023-11-05'
)
SELECT
  redemptionDate,
  redemptionCount
FROM
  MostRecentRedemptions
WHERE
  rn = 1
ORDER BY
  redemptionDate;

Answers to Questions  

1. Which date had the least number of redemptions and what was the redemption count?
Date: 2023-11-05
Redemption Count: 3702
2. Which date had the most number of redemptions and what was the redemption count?
Date: 2023-10-31
Redemption Count: 5003
3. What was the createDateTime for each redemptionCount in questions 1 and 2?
For 2023-11-05 (least redemptions): 2023-11-06 11:00:00 UTC
For 2023-10-31 (most redemptions): 2023-11-06 11:00:00 UTC
4. Is there another method you can use to pull back the most recent redemption count, by redemption date, for the date range 2023-10-30 to 2023-11-05, for retailer "ABC Store"?
Yes, another method involves using a subquery to get the maximum createDateTime for each redemptionDate and then joining it back to the main table. This would ensure that only the most recent record for each date is fetched.

Here’s the high-level approach:

Use a subquery to fetch the maximum createDateTime for each redemptionDate for the specified retailer and date range.
Join the result of this subquery with the tblRedemptions-ByDay table to retrieve the corresponding redemptionCount.
