# Using FusionCharts with FileMaker Pro 15

FileMaker, a powerful consumer database application, lets you create database solutions that work exactly as you want them to. Using FusionCharts Suite XT you render and manipulate your charts seamlessly across any application built using FileMaker Pro 15. With this you can add interactive JavaScript charts to your web viewer with the delight and comprehensiveness of the FusionCharts Suite.

This article talks about how you can render the charts and maps from FusionCharts using FileMaker Pro.

### Setting up the FusionCharts Suite XT

You can easily buy a license to use FusionCharts in any commercial application. The FusionCharts Suite trial version is also available to download for free with no feature restrictions (trial version will show an evaluation watermark).

Once you have bought the licensed or the trial version, installation of FusionCharts Suite XT merely involves copying and pasting the JavaScript files from the download package into your project folder. Thereafter, you can simply include the FusionCharts JavaScript library in your web applications and start building your charts, gauges, and maps.

FusionCharts provides you with an exhaustive gallery of live examples, hosted in JSFiddle along with the source code. This library serves as a great reference source to draw inspiration from.

### Creating a Project in FileMaker Pro

To render and manipulate charts using FileMaker, you first need to create a project in the FileMaker application.

Following are the basic steps that will help you in creating a project:
* Open the FileMaker application and click the File tab.
* In the navigation pane, select New Solution.
* Give a name to the application you are going to built and choose a directory for saving your project.
* Click Save. This will create:
    * A layout with the same name as that of your project
    * A table with the same name as that of your project

### Adding a Web-Viewer to your Layout

FileMaker lets you add a web viewer to display a web page on a layout. The address of the web page can either be a constant or a calculation based on the data in the current record.

To add a web viewer:
* From the status toolbar, click the web viewer tool
* Drag the crosshair to draw the web viewer
OR
From the Insert menu, click Web Viewer

### Adding FusionCharts to the Web Viewer

To render FusionCharts using Filemaker, add the FusionCharts JavaScript libraries to the web viewer using following steps:

* Click the File tab.
* From the navigation bar, point to Manage, and then select Database (or press CTRL+SHIFT+D). You will see a table with the same name of the layout, where you can add fields to the table by clicking on the table name.
* To add fields to the table, click menu/button/option. Name the first field say index_HTML and set its type to Calculation. The Specify Calculation dialog box opens.
* To render FusionCharts, create some basic fields such as chartType, container_id, chartWidth, chartHeight, dataFormat and dataSource and set their type to text.
* To make all these fields global:
    * Select a field and, from , click Option. The dialog box opens
    * Select the Storage option and select the checkbox for Use Global Storage
* In the Specify Calculation dialog box (created in step 3), write the source code that is executed to generate a HTML code (the 

HTML code is given below). This code is later appended to the Web-Viewer.

Click Web-Viewer. A window opens, where you have to write the table_name:: index_HTML in the web address (table name is same as that of your project name).

By following all the above steps web viewer gets linked to the calculation in the index_HTML.

### Adding Calculation to index_HTML

The code below is a template to be added in your index_HTML field. This calculation will render the HTML content for the chart:
```
"data:text/html," & ¶ &
"< !DOCTYPE html>" & ¶ &
"" & ¶ &
"" & ¶ &
//-->Embed FusionCharts Lib. from local file system
"<script src="\
"file:///drive-letter:/directory-name/fusionCharts.js\""
type = "\"text/javascript\"" > < /script>" & ¶ & "My first chart using FusionCharts Suite XT" & ¶ & "<script>/ / < ![CDATA[
            FusionCharts.ready(function() {
                var fusioncharts = new FusionCharts({
                    type: '"&<name of the table>::chartType&"',
                    renderAt: '"&<name of the table>::containerId&"',
                    width: '"&</name><name of the table>::chartWidth&"',
                    height: '"&</name><name of the table>::chartHeight&"',
                    dataFormat: '"&</name><name of the table>::dataFormat&"',
                    dataSource: {
                        "&table_name::dataSource&"
                    }
                });
                fusioncharts.render();
            }); < /script>" & ¶ &
            "</name>
            // ]]></script>" & ¶ & "< div id = \"chart-container\">FusionCharts XT will load here! " & ¶ & "" & ¶ & ""
```

There are 2 ways to include the FusionCharts library for the above code:
* Using the FusionCharts library link from the local folder:

```
<script src="file:///drive-letter:/directory-name/fusioncharts.js" type="text/javascript"></script>
```

* Using FusionCharts uploaded on a remote server:

```
<script src="link of fusioncharts.js" type="text/javascript"></script>
```

### Adding Data to the Fields

Add data for the following fields:

* __chartType__: The type of chart that you intend to plot. e.g. column 3D, column 2D, pie 2D etc.
* __chartId__: A unique ID for the chart, using which it will be recognized in the HTML page. Each chart on the page should have a unique Id.
* __chartWidth__: Intended width for the chart (in pixels), for example, 600
* __chartHeight__: Intended height for the chart (in pixels), for example, 450
* __containerId__: ID of the chart container, for example, chart-1
* __dataFormat__: Type of data used to render the chart, for example, json, xml.
* __dataSource__: Actual data for the chart, for example, {“chart”:{},”data”:[{“label”:”Jan”,”value”:”420000″}]}

To add data in the above fields:

Go to Browse mode and, from the status toolbar, select Table View. Add the field value one by one directly into the table.

### DataSource

Now, as you are all set with our library and web viewer, the last and the most important thing you need to render the chart is the datasource where you will pass your data to the chart. It will specify the source from where the data will be fetched, depending on the value passed to the dataFormat attribute.

__Note__: Stringify your JSON data before using it to render a chart

For example:
```
dataSource: "chart": {
        "theme": "fint",
        "caption": "Bakersfield Central - Total footfalls",
        "subCaption": "Last week",
        "xAxisName": "Day",
        "yAxisName": "No. of Visitors",
        "showValues": "0"
    },
    "data": [{
        "label": "Mon",
        "value": "15123"
    }, {
        "label": "Tue",
        "value": "14233"
    }, {
        "label": "Wed",
        "value": "25507"
    }, {
        "label": "Thu",
        "value": "9110"
    }, {
        "label": "Fri",
        "value": "15529"
    }, {
        "label": "Sat",
        "value": "20803"
    }, {
        "label": "Sun",
        "value": "19202"
    }]
```
In the above code, chart properties and the data values to be plotted are specified using the JSON format. The chart object is used to set the chart properties; the data object is used to specify the data labels (using the label attribute) and the data values (using the value attribute).

### Viewing the Chart

Now that all the setup is done, you don’t need to wait for long to view the output. All you need to do is:

* Click View and select Browse Mode (you can also do this by pressing Ctrl+B)

### Troubleshooting

While using FusionCharts with FileMaker Pro 15, you may face roadblocks or errors. Following are some troubleshooting suggestions:

* Escape the double quotes ” “, for the JSON data.
* The table name is by default same as that of your project.
* To add the libraries, always use their absolute path.
* Always add 3 forward slash (///) while adding libraries, before adding the path (for example: src=\”file:///C:/desktop/JS/fusionCharts.js\”)

Now, as you have created your first chart using FileMaker, here are some of the advantages of using FusionCharts Suite XT with FileMaker:

* Easy to Get Started
* Cross-platform (Mac OS X, Windows, iOS)
* Includes starter solutions
* There are many plugins available to extend the functionalities
* Has some neat built in tricks like built in graphs, tab controls, web viewers

### License

**FUSIONCHARTS:**

MIT License

Copyright (c) 2016 FusionCharts Technologies

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
