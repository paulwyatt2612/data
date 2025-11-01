# Github Data repository - PUBLIC

This data repo is PUBLIC.  Here are some notes to help connect to the various source formats that will appear here.
<br>

Paul Wyatt <br>
01/11/2025

## Connecting to web source xlsx files


### ✅ Correct way to connect your GitHub Excel file to Power BI

1. **Go to required Excel file on GitHub**
   URL - example - :

   ```
   https://github.com/paulwyatt2612/data/blob/main/Sales%20Dataset_Phantom%20%26%20Fang%20Outfitters.xlsx
   ```

2. **Click the “Download raw file” button** (or manually modify the URL):
   Replace

   ```
   github.com
   ```

   with

   ```
   raw.githubusercontent.com
   ```

   and remove `/blob/`.

   So your corrected URL becomes:

   ```
   https://raw.githubusercontent.com/paulwyatt2612/data/main/Sales%20Dataset_Phantom%20%26%20Fang%20Outfitters.xlsx
   ```

3. **Use that URL in Power BI:**

   * In **Power BI Desktop**, go to:
     **Home → Get Data → Web**
   * Paste the **raw.githubusercontent.com** URL.
   * Power BI will now properly detect and load the `.xlsx` file.

---

### ⚙️ Optional: If my repo is private

Power BI can’t directly authenticate to private GitHub repos using the “Web” connector.
You’ll need to either:

* Make the file public, **or**
* Download it locally and use **Get Data → Excel**, **or**
* Use **GitHub’s API with a token** (requires Power Query advanced setup).

---
### ✅ Power Query 'M' template script with parameters

1. Go to required Excel file on GitHub
1. Get URL
1. Identify each section and modify template as required
``` 
 ** Note **
 Using M template, there is no need to replace filename spaces with %20. So [Sales%20Dataset_Phantom%20&%20Fang%20Outfitters.xlsx] should be entered as Sales [Dataset_Phantom & Fang Outfitters.xlsx]
```

```m
let
    // Parameters for GitHub repository
    __GitHubUser = "paulwyatt2612",
    __GitHubRepo = "data",
    __GitHubBranch = "main",
    __FilePath = "Sales Dataset_Phantom & Fang Outfitters.xlsx",  // relative path in repo
    __SheetName = "Dim_Reseller",

    // URL-encode the file path for safety
    __EncodedFilePath = Uri.EscapeDataString(__FilePath),

    // Construct raw GitHub URL dynamically
    __FileURL = Text.Combine(
        {
            "https://raw.githubusercontent.com/",
            __GitHubUser, "/",
            __GitHubRepo, "/",
            __GitHubBranch, "/",
            __EncodedFilePath
        },
        ""
    ),

    // Load Excel workbook from constructed URL
    Source = Excel.Workbook(Web.Contents(__FileURL), null, true),

    // Access the specified worksheet
    Selected_Sheet = Source{[Item = __SheetName, Kind = "Sheet"]}[Data],

    // Promote first row to headers
    #"Promoted Headers" = Table.PromoteHeaders(Selected_Sheet, [PromoteAllScalars = true]),

    // Define column data types ** This should be actioned AFTER initial upload **
    #"Changed Type" = Table.TransformColumnTypes(
        #"Promoted Headers",
        {
            {"ResellerKey", Int64.Type},
            {"Reseller ID", type text},
            {"Reseller Business Type", type text},
            {"Reseller Business Name", type text},
            {"Reseller Business City", type text},
            {"Reseller Business State-Province", type text},
            {"Reseller Country", type text},
            {"Reseller Country Abbreviation", type text},
            {"Reseller Postal Code", type text},
            {"Reseller Company Size", type text}
        }
    )
in
    #"Changed Type"
```


