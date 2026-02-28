# Power BI Dashboard - Quick Reference

## 🚀 30-Second Start

```bash
# 1. Export data
python powerbi\create_powerbi_data.py

# 2. Open Power BI Desktop
# 3. Import all CSV files from powerbi/data_exports/
# 4. Follow DASHBOARD_GUIDE.md
```

---

## 📁 Files You Need

### Data Files (in `powerbi/data_exports/`)
1. ✅ `campaigns.csv` - 8 campaigns
2. ✅ `users.csv` - 10,000 users  
3. ✅ `events.csv` - 38,280 events
4. ✅ `campaign_performance.csv` - ROI metrics
5. ✅ `funnel_by_campaign.csv` - Conversion by campaign
6. ✅ `funnel_by_device.csv` - Conversion by device
7. ✅ `daily_metrics.csv` - Daily trends
8. ✅ `user_cohorts.csv` - Customer segments
9. ✅ `product_performance.csv` - Product analysis
10. ✅ `summary_metrics.csv` - Overall KPIs

### Guide Files
- 📖 `DASHBOARD_GUIDE.md` - Complete setup instructions
- 📊 `DAX_MEASURES.md` - All formulas (40+)
- 🎨 `QUICK_REFERENCE.md` - This file

---

## 🎯 Dashboard Pages

### Page 1: Executive Summary
**Purpose:** High-level business overview

**Key Visuals:**
- 4 KPI Cards (Users, Revenue, Conversion, ROI)
- Revenue trend line chart
- Top campaigns table
- Daily visitors area chart
- Device distribution donut chart

**Time to build:** 5 minutes

---

### Page 2: Funnel Analysis
**Purpose:** Conversion optimization

**Key Visuals:**
- Main funnel chart (Visit → Cart → Purchase)
- Funnel by campaign matrix
- Funnel by device bar chart
- Conversion rate cards

**Time to build:** 5 minutes

---

### Page 3: Campaign Performance
**Purpose:** Marketing ROI analysis

**Key Visuals:**
- Campaign ROI bar chart
- Cost vs Revenue scatter plot
- Channel performance column chart
- Campaign details table

**Time to build:** 5 minutes

---

### Page 4: Customer Insights
**Purpose:** Retention and LTV

**Key Visuals:**
- Customer segmentation donut
- LTV distribution histogram
- Repeat purchase rate by campaign
- Product category performance
- Retention metric cards

**Time to build:** 5 minutes

---

## 🔗 Relationships

```
campaigns ──(1)──→──(*)── users
    │                       │
    │                       │
   (1)                     (1)
    │                       │
    ↓                       ↓
   (*)                     (*)
  events ←────────────────┘
```

**Key relationships:**
- `campaigns[campaign_id]` → `users[acquisition_source]`
- `users[user_id]` → `events[user_id]`
- `campaigns[campaign_id]` → `events[campaign_id]`

---

## 📊 Top 10 DAX Measures

### Must-Have Metrics

```dax
1. Total Revenue = SUM(events[revenue])

2. Overall Conversion Rate = 
   DIVIDE([Total Purchasers], [Total Visitors], 0) * 100

3. ROI = 
   DIVIDE([Total Revenue] - [Total Marketing Cost], 
          [Total Marketing Cost], 0) * 100

4. Average Order Value = 
   DIVIDE([Total Revenue], [Total Purchases], 0)

5. Repeat Purchase Rate = 
   DIVIDE([Repeat Customers], [Total Purchasers], 0) * 100

6. Total Visitors = 
   CALCULATE(DISTINCTCOUNT(events[user_id]), 
             events[event_name] = "visit")

7. Total Purchasers = 
   CALCULATE(DISTINCTCOUNT(events[user_id]), 
             events[event_name] = "purchase")

8. Visit to Cart Rate = 
   DIVIDE([Total Cart Users], [Total Visitors], 0) * 100

9. ROAS = 
   DIVIDE([Total Revenue], [Total Marketing Cost], 0)

10. Cost Per Acquisition = 
    DIVIDE([Total Marketing Cost], [Total Users], 0)
```

---

## 🎨 Color Scheme

### Primary Colors
- **Blue:** #0078D4 (Primary brand)
- **Light Blue:** #00BCF2 (Secondary)
- **Sky Blue:** #50E6FF (Accent)

### Status Colors
- **Success:** #107C10 (Green) - ROI > 100%
- **Warning:** #FFB900 (Yellow) - ROI 0-100%
- **Error:** #D13438 (Red) - ROI < 0%

### Neutral Colors
- **Background:** #FFFFFF (White)
- **Grid:** #F3F2F1 (Light gray)
- **Borders:** #E1DFDD (Gray)

---

## ⚡ Quick Tips

### Data Import
1. Import CSVs in order (base tables first)
2. Check data types after import
3. Verify date columns are formatted correctly

### Relationships
1. Always use campaign_id as the key
2. Set cross-filter direction to "Both" for campaigns ↔ events
3. Make all relationships active

### DAX Measures
1. Create a "Measures" table for organization
2. Use DIVIDE() instead of / to avoid errors
3. Format measures appropriately (%, $, #,##0)

### Visualizations
1. Start with KPI cards for quick wins
2. Use conditional formatting for ROI metrics
3. Add data labels to important charts
4. Keep color scheme consistent

### Performance
1. Limit visuals per page to 6-8
2. Use aggregated views (campaign_performance, funnel_by_*)
3. Avoid calculated columns when measures work
4. Use DISTINCTCOUNT carefully

---

## 🐛 Troubleshooting

### Issue: Relationships not working
**Fix:** Check that column names match exactly and data types are correct

### Issue: Measures showing blank
**Fix:** Verify filter context and use CALCULATE() if needed

### Issue: Slow performance
**Fix:** Use pre-aggregated tables (campaign_performance, funnel_by_campaign)

### Issue: Incorrect totals
**Fix:** Use SUMX() or AVERAGEX() for row-level calculations

### Issue: Date filters not working
**Fix:** Create a separate Date table and relate it to all date columns

---

## 📋 Checklist

### Setup (5 min)
- [ ] Run `python powerbi\create_powerbi_data.py`
- [ ] Open Power BI Desktop
- [ ] Import all 10 CSV files
- [ ] Create relationships

### Measures (5 min)
- [ ] Create "Measures" table
- [ ] Add top 10 DAX measures
- [ ] Format measures (%, $)
- [ ] Test calculations

### Page 1 (5 min)
- [ ] Add 4 KPI cards
- [ ] Create revenue trend chart
- [ ] Add top campaigns table
- [ ] Add device donut chart

### Page 2 (5 min)
- [ ] Create funnel visualization
- [ ] Add funnel by campaign matrix
- [ ] Add funnel by device chart
- [ ] Add conversion rate cards

### Page 3 (5 min)
- [ ] Create ROI bar chart
- [ ] Add cost vs revenue scatter
- [ ] Add channel performance chart
- [ ] Add campaign details table

### Page 4 (5 min)
- [ ] Create segmentation donut
- [ ] Add LTV histogram
- [ ] Add repeat purchase chart
- [ ] Add product performance chart

### Polish (5 min)
- [ ] Apply color theme
- [ ] Add slicers (date, campaign, device)
- [ ] Add conditional formatting
- [ ] Test all interactions
- [ ] Save as .pbix file

---

## 🎯 Expected Results

### Key Metrics
- **Total Users:** 10,000
- **Total Revenue:** ~$670,000
- **Overall Conversion:** ~40%
- **Marketing ROI:** ~480%

### Top Insights
1. Email Newsletter has best ROI (1,237%)
2. Mobile conversion is similar to desktop (~40%)
3. 20% repeat purchase rate
4. Electronics category drives most revenue

---

## 📚 Additional Resources

- **Full Guide:** [DASHBOARD_GUIDE.md](file:///c:/Users/Isha/.gemini/antigravity/scratch/product-analytics-project/powerbi/DASHBOARD_GUIDE.md)
- **DAX Reference:** [DAX_MEASURES.md](file:///c:/Users/Isha/.gemini/antigravity/scratch/product-analytics-project/powerbi/DAX_MEASURES.md)
- **SQL Queries:** [sql/](file:///c:/Users/Isha/.gemini/antigravity/scratch/product-analytics-project/sql/)
- **Data Exports:** [powerbi/data_exports/](file:///c:/Users/Isha/.gemini/antigravity/scratch/product-analytics-project/powerbi/data_exports/)

---

**Total Time:** ~30 minutes for complete dashboard
**Difficulty:** Beginner to Intermediate
**Prerequisites:** Power BI Desktop installed

🎉 **You're ready to build!** Start with Page 1 and work your way through.
