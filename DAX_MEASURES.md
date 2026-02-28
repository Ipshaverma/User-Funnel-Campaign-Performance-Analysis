# DAX Measures Reference

Complete list of DAX measures for the Product Analytics Dashboard

## 📊 Core Metrics

### User Metrics

```dax
Total Users = DISTINCTCOUNT(users[user_id])
```

```dax
Total Visitors = 
CALCULATE(
    DISTINCTCOUNT(events[user_id]), 
    events[event_name] = "visit"
)
```

```dax
Total Cart Users = 
CALCULATE(
    DISTINCTCOUNT(events[user_id]), 
    events[event_name] = "add_to_cart"
)
```

```dax
Total Purchasers = 
CALCULATE(
    DISTINCTCOUNT(events[user_id]), 
    events[event_name] = "purchase"
)
```

### Event Metrics

```dax
Total Events = COUNTROWS(events)
```

```dax
Total Visits = 
CALCULATE(
    COUNTROWS(events), 
    events[event_name] = "visit"
)
```

```dax
Total Cart Additions = 
CALCULATE(
    COUNTROWS(events), 
    events[event_name] = "add_to_cart"
)
```

```dax
Total Purchases = 
CALCULATE(
    COUNTROWS(events), 
    events[event_name] = "purchase"
)
```

---

## 💰 Revenue Metrics

```dax
Total Revenue = 
SUMX(
    FILTER(events, events[event_name] = "purchase"),
    events[revenue]
)
```

```dax
Average Order Value = 
DIVIDE([Total Revenue], [Total Purchases], 0)
```

```dax
Revenue Per Visitor = 
DIVIDE([Total Revenue], [Total Visitors], 0)
```

```dax
Revenue Per User = 
DIVIDE([Total Revenue], [Total Users], 0)
```

---

## 📈 Conversion Metrics

```dax
Visit to Cart Rate = 
DIVIDE([Total Cart Users], [Total Visitors], 0) * 100
```

```dax
Cart to Purchase Rate = 
DIVIDE([Total Purchasers], [Total Cart Users], 0) * 100
```

```dax
Overall Conversion Rate = 
DIVIDE([Total Purchasers], [Total Visitors], 0) * 100
```

```dax
Purchase Rate = 
DIVIDE([Total Purchasers], [Total Users], 0) * 100
```

---

## 💸 Marketing Metrics

```dax
Total Marketing Cost = SUM(campaigns[cost])
```

```dax
ROI = 
DIVIDE(
    [Total Revenue] - [Total Marketing Cost], 
    [Total Marketing Cost], 
    0
) * 100
```

```dax
ROAS = 
DIVIDE([Total Revenue], [Total Marketing Cost], 0)
```

```dax
Cost Per Acquisition = 
DIVIDE([Total Marketing Cost], [Total Users], 0)
```

```dax
Cost Per Purchase = 
DIVIDE([Total Marketing Cost], [Total Purchasers], 0)
```

```dax
Revenue Per Dollar Spent = 
DIVIDE([Total Revenue], [Total Marketing Cost], 0)
```

---

## 🔄 Retention Metrics

```dax
Repeat Purchase Rate = 
VAR RepeatCustomers = 
    CALCULATE(
        DISTINCTCOUNT(user_cohorts[user_id]),
        user_cohorts[customer_segment] = "Repeat Customer"
    )
VAR TotalPurchasers = 
    CALCULATE(
        DISTINCTCOUNT(user_cohorts[user_id]),
        user_cohorts[customer_segment] <> "Non-Purchaser"
    )
RETURN
    DIVIDE(RepeatCustomers, TotalPurchasers, 0) * 100
```

```dax
Average Lifetime Value = 
AVERAGE(user_cohorts[lifetime_value])
```

```dax
Total Lifetime Value = 
SUM(user_cohorts[lifetime_value])
```

```dax
One-Time Buyers = 
CALCULATE(
    DISTINCTCOUNT(user_cohorts[user_id]),
    user_cohorts[customer_segment] = "One-time Buyer"
)
```

```dax
Repeat Customers = 
CALCULATE(
    DISTINCTCOUNT(user_cohorts[user_id]),
    user_cohorts[customer_segment] = "Repeat Customer"
)
```

```dax
Non-Purchasers = 
CALCULATE(
    DISTINCTCOUNT(user_cohorts[user_id]),
    user_cohorts[customer_segment] = "Non-Purchaser"
)
```

---

## 📅 Time-Based Metrics

```dax
Daily Average Revenue = 
AVERAGEX(
    VALUES(daily_metrics[date]),
    [Total Revenue]
)
```

```dax
Daily Average Visitors = 
AVERAGE(daily_metrics[daily_visitors])
```

```dax
Revenue Growth = 
VAR CurrentRevenue = [Total Revenue]
VAR PreviousRevenue = 
    CALCULATE(
        [Total Revenue],
        DATEADD(daily_metrics[date], -1, MONTH)
    )
RETURN
    DIVIDE(CurrentRevenue - PreviousRevenue, PreviousRevenue, 0) * 100
```

---

## 🎯 Campaign-Specific Metrics

```dax
Campaign Users = 
CALCULATE(
    DISTINCTCOUNT(users[user_id]),
    USERELATIONSHIP(users[acquisition_source], campaigns[campaign_id])
)
```

```dax
Campaign Revenue = 
CALCULATE(
    [Total Revenue],
    USERELATIONSHIP(events[campaign_id], campaigns[campaign_id])
)
```

```dax
Campaign Conversion Rate = 
VAR CampaignVisitors = 
    CALCULATE(
        [Total Visitors],
        USERELATIONSHIP(events[campaign_id], campaigns[campaign_id])
    )
VAR CampaignPurchasers = 
    CALCULATE(
        [Total Purchasers],
        USERELATIONSHIP(events[campaign_id], campaigns[campaign_id])
    )
RETURN
    DIVIDE(CampaignPurchasers, CampaignVisitors, 0) * 100
```

---

## 🛍️ Product Metrics

```dax
Total Products Viewed = 
CALCULATE(
    DISTINCTCOUNT(events[product_id]),
    events[event_name] = "visit"
)
```

```dax
Total Products in Cart = 
CALCULATE(
    DISTINCTCOUNT(events[product_id]),
    events[event_name] = "add_to_cart"
)
```

```dax
Total Products Purchased = 
CALCULATE(
    DISTINCTCOUNT(events[product_id]),
    events[event_name] = "purchase"
)
```

---

## 📱 Device Metrics

```dax
Mobile Conversion Rate = 
CALCULATE(
    [Overall Conversion Rate],
    users[device] = "mobile"
)
```

```dax
Desktop Conversion Rate = 
CALCULATE(
    [Overall Conversion Rate],
    users[device] = "desktop"
)
```

```dax
Tablet Conversion Rate = 
CALCULATE(
    [Overall Conversion Rate],
    users[device] = "tablet"
)
```

---

## 🎨 Conditional Formatting Measures

```dax
ROI Color = 
SWITCH(
    TRUE(),
    [ROI] > 100, "#107C10",  // Green
    [ROI] > 0, "#FFB900",     // Yellow
    "#D13438"                 // Red
)
```

```dax
Conversion Color = 
SWITCH(
    TRUE(),
    [Overall Conversion Rate] > 40, "#107C10",  // Green
    [Overall Conversion Rate] > 20, "#FFB900",  // Yellow
    "#D13438"                                   // Red
)
```

---

## 📊 Advanced Calculated Columns

### In users table:

```dax
Days Since Signup = 
DATEDIFF(users[signup_date], TODAY(), DAY)
```

```dax
User Age Group = 
SWITCH(
    TRUE(),
    [Days Since Signup] <= 30, "New (0-30 days)",
    [Days Since Signup] <= 90, "Active (31-90 days)",
    [Days Since Signup] <= 180, "Established (91-180 days)",
    "Veteran (180+ days)"
)
```

### In events table:

```dax
Event Month = 
FORMAT(events[event_date], "YYYY-MM")
```

```dax
Event Day of Week = 
FORMAT(events[event_date], "dddd")
```

```dax
Event Hour = 
HOUR(events[event_date])
```

---

## 🔢 Ranking Measures

```dax
Campaign Rank by Revenue = 
RANKX(
    ALL(campaigns[campaign_name]),
    [Total Revenue],
    ,
    DESC,
    DENSE
)
```

```dax
Campaign Rank by ROI = 
RANKX(
    ALL(campaigns[campaign_name]),
    [ROI],
    ,
    DESC,
    DENSE
)
```

---

## 📈 Trend Measures

```dax
Revenue Trend = 
VAR CurrentRevenue = [Total Revenue]
VAR AverageRevenue = [Daily Average Revenue]
RETURN
    SWITCH(
        TRUE(),
        CurrentRevenue > AverageRevenue * 1.1, "↑ Above Average",
        CurrentRevenue < AverageRevenue * 0.9, "↓ Below Average",
        "→ Average"
    )
```

---

## 💡 Usage Tips

1. **Create a Measures table:** Right-click in Fields pane → New Table → Name it "Measures"
2. **Organize measures:** Use Display Folders to group related measures
3. **Format measures:** Set appropriate number formats (%, $, #,##0)
4. **Hide base columns:** Hide raw columns used only in measures
5. **Test measures:** Always validate calculations with known values

---

## 🎯 Quick Copy Instructions

1. Open Power BI Desktop
2. Go to **Modeling** tab
3. Click **New Measure**
4. Copy and paste each DAX formula
5. Press Enter to save
6. Repeat for all measures

---

**Total Measures:** 40+
**Categories:** Core, Revenue, Conversion, Marketing, Retention, Time-Based, Campaign, Product, Device

All measures are optimized for performance and follow Power BI best practices.
