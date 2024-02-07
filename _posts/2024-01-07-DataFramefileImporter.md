---
layout: post
title:  "Streamlining CSV Data Import with Swift TabularData, FileImporter and FileExporter"
date:   2024-01-07 00:00:42 +0530
categories: Swift SwiftUI iOS macOS
---
Handling CSV files in Swift can be a straightforward process with the help of TabularData. This framework provides a simple and efficient way to read and write CSV files. With TabularData, you can easily load and save CSV files with fileImporter and fileExporter. Here is how I set them up.

# FileImporter
When performing fileImporter, make sure the loading function is between if **url.startAccessingSecurityScopedResource()** and **url.stopAccessingSecurityScopedResource()** to make sure the program can access the file.

```swift
if url.startAccessingSecurityScopedResource(){
//The functions
}
url.stopAccessingSecurityScopedResource()
```

The following is the .fileImporter implementation.

```swift
.fileImporter(isPresented: $showFileImporter, allowedContentTypes: [UTType.commaSeparatedText], onCompletion: { result in
            switch result{
            case .success(let url):
                let options = CSVReadingOptions(hasHeaderRow: true, delimiter: ",") //getting file permission
                if url.startAccessingSecurityScopedResource(){
                    do{
                        myData = try DataFrame.init(contentsOfCSVFile: url, options:options)
                    } catch {
                        //print(error.localizedDescription)
                        print(error.localizedDescription)
                    }
                }
                url.stopAccessingSecurityScopedResource()
                
            case .failure(let error):
                print(error.localizedDescription)
            }
        })
```

# fileExporter

In order to use fileExporter, we need to create a class that conforms to the FileDocument protocol. In fileWrapper, I have used DataFrame.csvRepresentation() to covert my DataFrame to a CSV data instance of the data frame type. Therefore fileWrapper can export the file properly.

## Set up a class to handle CSV documents
Remember to include SwiftUI, TabularData, and UniformTypeIdentifiers when creating a swift file for our FileDocument class.

```swift
import Foundation
import SwiftUI
import TabularData
import UniformTypeIdentifiers

struct MyCSV: FileDocument {
    // tell the system we support only csv files
    static var readableContentTypes = [UTType.commaSeparatedText]
    static var writableContentTypes = [UTType.commaSeparatedText]

    // by default our DataFrame is empty
    var dataframe: DataFrame = DataFrame()

    // a simple initializer that creates new, empty documents
    init(initialData: DataFrame = DataFrame()) {
        dataframe = initialData
    }

    // this initializer loads data that has been saved previously
    init(configuration: ReadConfiguration) throws {
        if let data = configuration.file.regularFileContents {
            dataframe = try! DataFrame(csvData: data)
        }
    }

    // this will be called when the system wants to write our data to disk
    func fileWrapper(configuration: WriteConfiguration) throws -> FileWrapper {
        let data = try! dataframe.csvRepresentation()
        return FileWrapper(regularFileWithContents: data)
    }
}
```

After creating the MyCSV class, we can start use fileExporter to save our dataframe. First we need to create a MyCSV object in order to use fileExporter.

```swift
let myCSV = MyCSV(initialData: myData)
```

Then we can start use fileExporter as follow:

```swift
.fileExporter(isPresented: $showFileExporter, document: myCSV, onCompletion: {result in
            switch result {
            case .success(let url):
                print(url)
            case .failure(let error):
                print(error.localizedDescription)
            }
        })
```
[Here is the link to the full playground demo.](https://github.com/hsuanchunlin/MediumDemo/blob/main/TabularDatafileImporter-MacOS.playground/Contents.swift)

# References
1. [https://www.hackingwithswift.com/quick-start/swiftui/how-to-export-files-using-fileexporter](https://www.hackingwithswift.com/quick-start/swiftui/how-to-export-files-using-fileexporter)
2. [https://stackoverflow.com/questions/67997894/swiftui-fileexporter-with-a-csv-file-in-the-view](https://stackoverflow.com/questions/67997894/swiftui-fileexporter-with-a-csv-file-in-the-view)