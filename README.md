# Connecting to web sources files
Paul Wyatt
01/11/2025

### ✅ Correct way to connect your GitHub Excel file to Power BI

1. **Go to your Excel file on GitHub**
   URL:

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

### ⚙️ Optional: If your repo is private

Power BI can’t directly authenticate to private GitHub repos using the “Web” connector.
You’ll need to either:

* Make the file public, **or**
* Download it locally and use **Get Data → Excel**, **or**
* Use **GitHub’s API with a token** (requires Power Query advanced setup).

---

Would you like me to show you the Power Query `M` code version that automatically fetches the file from GitHub (and could later handle tokens if private)?
