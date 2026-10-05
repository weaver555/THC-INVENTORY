# THC Inventory Management

A responsive, static-first restaurant inventory website for GitHub Pages. It includes daily counts, production and stock-in records, Loyverse CSV import, Recipe/BOM mappings, actual-versus-expected usage, variance flags, replenishment suggestions, best sellers, reports, an activity log, and a built-in item/recipe guide.

## Open the demo

Open `index.html` in a browser, or publish the repository with GitHub Pages. The app has sample data so the screens are populated immediately. All changes are stored in that browser's local storage. They do not sync to another device or another team member yet. Use **Export data** on Reports to download a JSON backup.

## Publish with GitHub Pages

1. Sign in to GitHub and create a new repository, for example `thc-inventory`.
2. Upload `index.html`, `styles.css`, `app.js`, and this README to the repository's root. Keep the three app files together.
3. Open the repository's **Settings → Pages**.
4. Under **Build and deployment**, select **Deploy from a branch**.
5. Select the `main` branch and `/ (root)`, then select **Save**.
6. Wait for the Pages deployment to finish. GitHub will show the website URL in Settings → Pages. It will look like `https://YOUR-USERNAME.github.io/thc-inventory/`.

To update the live site, edit/upload the files on GitHub. GitHub Pages republishes the change automatically. No build command is required.

## Add or edit inventory items

In the website, choose **Master inventory → Add inventory item**. Enter the item name, category, count unit, reorder level, critical level, and target stock, then save. Edit an existing row with the pencil icon; the circle icon deactivates/reactivates an item. Deactivated items remain in historical records but disappear from new count and movement forms. Duplicate item names are blocked.

Use one unit consistently (for example, count fries in kg and enter received stock in kg). Set critical level at or below reorder level and target stock at or above reorder level. The guide is also available inside the app under **Quick guide**.

## Add Recipe / BOM mappings

Choose **Recipe / BOM → Add recipe mapping**. Enter the menu item name exactly as Loyverse exports it, choose one inventory ingredient, and enter the quantity used for one serving. Repeat for each ingredient in the recipe. Existing duplicate menu-item/ingredient pairs are blocked. Expected usage is the sum of `quantity sold × quantity per serving` for each ingredient.

## Daily operating flow

1. Production staff record prepared quantities under **Production & stock in**.
2. A manager records purchases/transfers received under **Stocks in**.
3. At night, enter physical ending counts under **Daily inventory** and save them.
4. That saved ending quantity becomes the next calendar day's beginning count. Beginning is zero until an earlier ending count exists.
5. Import a Loyverse CSV under **Loyverse sales**. The importer asks you to match the menu item, quantity, and optional date columns. A selected default date is used when there is no date column.
6. Review **Sales reconciliation**. Actual used is `beginning + production + stocks in − physical ending`; expected used is `sales × recipe`. A flag is raised when the difference exceeds the default 10% tolerance. The app does not adjust counts automatically.
7. Use **Replenishment** for a suggested top-up to each item's target level. Confirm suggestions before ordering.

For manual entry (handy for trying the app), open Loyverse sales and choose **Add sale**. Best sellers use the selected/current day's sales.

## What this version stores

This is a working single-browser prototype: browser local storage is the current persistence adapter. The code keeps the data model and calculation/rendering functions in `app.js` separated so persistence can be replaced by a Google Sheets / Apps Script adapter or a database API. GitHub Pages serves static files only; it does not provide a database, private login, or shared records. Do not treat this demo as a multi-device, permission-controlled production system.

`backend/Code.gs` is an Apps Script starting point. It stores the current app data JSON in a Google Sheet and checks the caller's Google account email against an allow-list. It is intentionally separate from the Pages frontend: Apps Script web apps have deployment and browser cross-origin behavior that must be configured and verified for the organization's Google account setup before connecting it. Do not publish the sheet or deploy a public unauthenticated write endpoint. A later adapter can replace the local `loadState()` / `persist()` functions in `app.js` while leaving the app's calculations and interface in place.

## Data backup and reset

Use **Reports → Export data** to save a JSON backup and **Activity log → Export log** for a CSV audit extract. Clearing the browser's site data removes this browser's saved records. Sample items and recipe mappings are loaded again if local storage is cleared.

## Project files

- `index.html` — app layout and pages
- `styles.css` — responsive visual styling
- `app.js` — sample data, local storage, calculations, forms, import, exports, and rendering
- `backend/Code.gs` — optional Google Apps Script persistence starter
