# R-tips-tricks

##### Install a package
`install.packages("ggplot2")`

##### Load a package
`library(ggplot2)`

##### Read a CSV file
`csv_file = read.csv("csv_file_path", header = FALSE)`

##### For loop
```R
for (index in 1:25){
}
```
##### Combine two data frames
`combined = rbind(first, second)`

##### Filter a data frame
`filetered = complete[which(complete$columnName > 1), ]`

##### Print number of rows in a data frame
`nrow(data)`

##### Plot a histogram (requires package ggplot2)
`qplot(column_name, data=data, bins = 100, main="title",  ylim=c(0, 1000))`


##### Plot overlaying histograms based on column values
```
ggplot(data, aes(x=column_to_plot, fill=column_for_visual_separation)) + geom_histogram(alpha=0.5, position="identity", binwidth=10)
```

##### Add a column to existing data frame
`data$column_name = 'some_constant_value'`

##### box plot
`boxplot(column_to_plot ~ column_for_unique_plots, data=data, ylab = "title")`

##### Create a new column using existing columns
`data$new_column = as.numeric(data$something/data$something_else)`

##### Replace column values
`data$some_column[data$some_column == 'old_value'] <- 'new_value'`

##### format column as date time
`data$start_time = as.POSIXct(data$start_time, format="%d/%m/%Y %H:%M:%S")`

##### unfactor a column
```
library(varhandle)
data$column_name = unfactor(data[, "column_name"])

# then do the filtering
filtered = data[which(data$start_time >= "2017-02-18 16:00:00"), ]
```
##### In operator
`data$column_name %in% some_other_data[, "column_name"]))`

##### Take subset of data using Not In operator
```
data_subset = subset(data_to_subset, !(data_to_subset$column_name %in% some_other_data[, "column_name"]))
```

##### Concatenate two columns
`data$concatenated = paste(data$column_one, data$column_two, sep="-")`

##### Extract column as data frame
new_data_frame = as.data.frame( data_frame[,"column_name"], drop=false)

##### Plot multiple histograms side-by-side
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
