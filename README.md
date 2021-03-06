# R-tips-tricks

#### Install a package
`install.packages("ggplot2")`

#### Load a package
`library(ggplot2)`

#### Read a CSV file
`csv_file = read.csv("csv_file_path", header = FALSE)`

#### Write to a CSV file
`write.csv(data_frame, file = "/file/path.csv", row.names=FALSE)`

#### Print first 5 rows of a data frame
`head(data_frame)`

#### Print last 5 rows of a data frame
`tail(data_frame)`

#### For loop
```R
for (index in 1:25){
}
```

#### Force R not to use exponential notation
`options(scipen = 999)`

#### Combine two data frames (vertically, row-wise)
`combined = rbind(first, second)`

#### Combine two data frames (horizontally, column-wise)
`combined = cbind(first, second)`

#### Remove duplicate rows from a data frame
`no_duplicate_rows = unique(data_frame)`

#### Filter a data frame
`filetered = complete[which(complete$columnName > 1), ]`

#### Print number of rows in a data frame
`nrow(data)`

#### Print summary of a column in a data frame
`summary(data_frame$column_name)`

#### Add a column to existing data frame
`data$column_name = 'some_constant_value'`

#### Delete a column from data frame
```
data_frame = within(data_frame, rm(column_name))
# or delete multiple
data_frame = within(data_frame, rm(column_1, column_2))
```

#### Create a new column using existing columns
`data$new_column = as.numeric(data$something/data$something_else)`

#### Replace column values
`data$some_column[data$some_column == 'old_value'] <- 'new_value'`

#### Format column as date time
```
data$start_time = as.POSIXct(data$start_time, format="%d/%m/%Y %H:%M:%S")
# then do the filtering
filtered = data[which(data$start_time >= "2017-02-18 16:00:00"), ]
```

#### Unfactor a column
```
library(varhandle)
data$column_name = unfactor(data[, "column_name"])
```
#### Find percentage contribution of each distinct value in a column
`as.data.frame(prop.table(table(data_frame$column_name)) * 100)`

#### Find data types of data frame
`sapply(data_frame, class)`

#### Find distinct values
`unique(data_frame$column_name)`

#### Rename column
`colnames(data_frame)[column_index] <- "new_name"`

#### In operator
`data$column_name %in% some_other_data[, "column_name"]))`

#### Take subset of data using Not In operator
```
data_subset = subset(data_to_subset, !(data_to_subset$column_name %in% some_other_data[, "column_name"]))
```

#### Concatenate two columns
`data$concatenated = paste(data$column_one, data$column_two, sep="-")`

#### Extract column as data frame
`new_data_frame = as.data.frame( data_frame[,"column_name"], drop=false)`

#### Stratified sampling
```
# you would need caTools package
install.packages("caTools")
library(caTools)
sampled_data = sample.split(data_frame$column_for_sampling, SplitRatio=0.7)
training_set = sampled_data[sampled_data,]
test_set = sampled_data[!sampled_data,]
```

#### Convert discrete values e.g. TCP/UDP, into continous (1/0) columns
```
new_columns = model.matrix(~ column_to_convert - 1, data= data_frame ))
# merge new columns with existing data frame
converted_data_frame = cbind(data_frame, new_columns)
# delete source column
converted_data_frame = within(converted_data_frame, rm(column_to_convert))
```

#### Keep those columns in a data frame that exists in another
```
# we want to keep only those columns in data_frame_1 which are
# present in data_frame_2.
# find ids of common columns
common_columns = as.data.frame(which(colnames(data_frame_1) %in% colnames(data_frame_2)))
# give this column of ids a new name using index
colnames(common_columns)[1] <- "id"
# keep only common columns
data_frame_1 = data_frame_1[, common_columns$id]
```

##### Scatter plot
```
plot(data_frame$column1, data_frame$column2, main="title", xlab="x-label ", ylab="y-label", pch=19, col=factor(data_frame$color_column))
```

#### Plot a histogram (requires package ggplot2)
`qplot(column_name, data=data, bins = 100, main="title",  ylim=c(0, 1000))`

#### Plot overlaying histograms based on column values
```
ggplot(data, aes(x=column_to_plot, fill=column_for_visual_separation)) +
geom_histogram(alpha=0.5, position="identity", binwidth=10)
```

#### Box plot
`boxplot(column_to_plot ~ column_for_unique_plots, data=data, ylab = "title")`

#### Plot multiple histograms side-by-side
Function to do the multi-plotting
```
multiplot <- function(..., plotlist=NULL, cols) {
    require(grid)

    # Make a list from the ... arguments and plotlist
    plots <- c(list(...), plotlist)

    numPlots = length(plots)

    # Make the panel
    plotCols = cols  # Number of columns of plots
    plotRows = ceiling(numPlots/plotCols) # Number of rows needed, calculated from # of cols

    # Set up the page
    grid.newpage()
    pushViewport(viewport(layout = grid.layout(plotRows, plotCols)))
    vplayout <- function(x, y)
        viewport(layout.pos.row = x, layout.pos.col = y)

    # Make each plot, in the correct location
    for (i in 1:numPlots) {
        curRow = ceiling(i/plotCols)
        curCol = (i-1) %% plotCols + 1
        print(plots[[i]], vp = vplayout(curRow, curCol ))
    }

}
```
Plot the data using multiplot function
```
plot1 = qplot(column_name, data=data1, bins = 100, main="title1",  ylim=c(0, 1000), col="red")
plot2 = qplot(column_name, data=data2, bins = 100, main="title2", ylim=c(0, 1000), col="green")
multiplot(plot1, plot2, cols=2)
```
