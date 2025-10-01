# Website Updates

## Latest Features (New!)

### Data Granularity Slider

A new granularity slider allows users to control the aggregation level of the dataset, providing flexibility in viewing data at different time scales:

**Available Granularity Options:**
- **30 min** (default) - Original data resolution, no aggregation
- **1 hr** - Aggregates data into 1-hour buckets
- **3 hr** - Aggregates data into 3-hour buckets
- **6 hr** - Aggregates data into 6-hour buckets
- **12 hr** - Aggregates data into 12-hour buckets
- **1 day** - Aggregates data into daily buckets
- **7 days** - Aggregates data into weekly buckets

**How it Works:**
- The slider is located in the date range section
- Data points within each time bucket are combined by summing deltas
- The last cumulative value in each bucket is preserved
- Works seamlessly with all visualization modes and date range filters
- Real-time label updates show the current granularity setting

**Use Cases:**
- View detailed short-term trends with 30 min or 1 hr granularity
- Analyze medium-term patterns with 3 hr, 6 hr, or 12 hr buckets
- Examine long-term trends with daily or weekly aggregation
- Reduce noise in data by using coarser granularity

### Enhanced Data Visualization Options

#### 1. Summary Statistics Panel
A new statistics panel displays key metrics about the data:
- **Total Characters**: Number of characters with data in the selected config
- **Total Plays**: Sum of all plays across all characters
- **Most Popular**: Character with the highest total plays
- **Fastest Growing**: Character with the highest growth rate (slope)
- **Avg per Character**: Average plays per character

The stats panel appears automatically when viewing any visualization mode and updates based on the selected date range.

#### 2. Market Share (Pie Chart) View
A new donut chart visualization showing the relative popularity of characters:
- Visual representation of each character's share of total plays
- Vibrant color palette for easy distinction
- Tooltips show exact play counts and percentage of total
- Works with the Top N filter to show only the most popular characters

#### 3. Character Comparison Mode
Select multiple characters (2-6) to compare their cumulative play trends:
- Multi-select list (hold Ctrl/Cmd to select multiple)
- Side-by-side line chart comparison
- Each character gets a distinct color
- Perfect for analyzing relative popularity over time

#### 4. Top N Filter
Available for the following views:
- All Characters (Lines)
- Slope Chart (Trends)
- Market Share (Pie)

Filter options:
- All Characters
- Top 5
- Top 10 (default)
- Top 15
- Top 20

This filter helps focus on the most popular characters and reduces visual clutter in complex charts.

## Previous Features

### 1. View Modes
The website includes 6 different visualization modes:

- **Single Character**: The original view showing cumulative plays over time for a single character
- **All Characters**: Bar chart showing total plays for all characters (ranked by popularity)
- **All Characters (Lines)**: Multi-line chart showing rate of change (plays per hour) for all characters over time
- **Slope Chart (Trends)**: Bar chart showing growth rate (linear regression slope) for each character
- **Market Share (Pie)**: Donut chart showing relative popularity percentages
- **Compare Characters**: Multi-select comparison of 2-6 characters' cumulative trends

### 2. Date Range Selector
Added a date range filter that works across all view modes:

- **Start Date**: Filter data from this date onwards
- **End Date**: Filter data up to this date
- **Reset Range**: Button to clear the date filters and show all data

The date inputs automatically set their min/max values based on the available data in the selected config.

### 3. All Characters Line Chart Details
The new "All Characters (Lines)" view shows the rate of change over time:

- **Normalized Timestamps**: Collects all unique timestamps across all characters and fills in missing data points
- **Zero-Delta Assumption**: If a character has no data at a timestamp, it assumes delta=0 (carries forward the last cumulative value)
- **Rate Calculation**: Calculates plays per hour between consecutive data points
- Each character gets a unique color from a 20-color palette
- Shows all characters on the same chart for easy comparison
- Helps identify which characters are gaining or losing popularity over time

This normalization ensures fair comparison - characters without activity during a time period will show 0 rate of change, rather than having gaps in the chart.

### 4. Dynamic UI
- The character selector is automatically hidden when viewing "All Characters" modes
- Date range filtering applies to all visualization modes
- Changing the date range automatically refreshes the current view

## Technical Implementation

### Data Filtering
- `filterDataByDateRange()`: Filters time series data based on selected date range
- `updateDateRangeLimits()`: Sets date input constraints based on available data

### Visualization Functions
- `updateSingleCharacterChart()`: Original single character view
- `updateAllCharactersChart()`: Bar chart of total plays
- `updateAllCharactersLineChart()`: Multi-line rate of change chart
- `updateSlopeChart()`: Growth rate comparison

### Rate of Change Calculation
The "All Characters (Lines)" view calculates the rate of change as:

1. **Timestamp Normalization**: First, collect all unique timestamps from all characters
2. **Fill Missing Data**: For each character, if they don't have data at a timestamp, carry forward the last cumulative value (delta = 0)
3. **Calculate Rate**: For each normalized time period:
```javascript
const timeDiff = (new Date(currPoint.t) - new Date(prevPoint.t)) / (1000 * 60 * 60); // hours
const valueDiff = currPoint.c - prevPoint.c;
const rate = timeDiff > 0 ? valueDiff / timeDiff : 0; // plays per hour
```

This normalization ensures that:
- All characters have data points at all timestamps
- Missing activity periods correctly show as 0 rate of change
- Characters can be fairly compared on the same timeline

## Usage

1. Select a config from the dropdown
2. Choose your preferred view mode
3. (Optional) Set a date range to focus on a specific time period
4. For single character view, select a character from the dropdown

The visualizations update automatically when you change any of these settings.
