# AstrologyWebsiteTamil

# திருக்கணித ஜாதகக் கணக்கீடு (Tamil Thirukanitha Horoscope)
Static web app to calculate and display:
- Jathaga Kattam (Rasi/D1)
- Navamsa Kattam (D9)
- Vimshottari Dasha Irupu

All output labels are in Tamil. Uses Drik (Thirukanitha) astronomy with Lahiri (Chitrapaksha) ayanamsa.

Live demo
- If you host via GitHub Pages, your URL will be: https://YOUR_USERNAME.github.io/YOUR_REPO/
- Replace YOUR_USERNAME and YOUR_REPO after you deploy.

Features
- Tamil UI and output (signs, planets, nakshatra, tithi, dasha labels)
- South-Indian chart layout for D1 and D9
- Vimshottari Dasha sequence with balance at birth
- Panchanga snippet (Nakshatra + Pada, Tithi)
- Place search via OpenStreetMap Nominatim (lat/lon auto-fill)
- Runs 100% in the browser (no server), easy to host on GitHub Pages

Tech stack
- HTML/CSS/JavaScript (single file: index.html)
- Astronomy Engine (MIT license) for planetary longitudes
- Custom Lahiri ayanamsa conversion
- Nominatim (OpenStreetMap) for geocoding

Accuracy notes
- Uses Drik calculations and a standard Lahiri ayanamsa approximation. Good for charts/dashas in most cases.
- If you need professional-grade agreement with commercial panchangam apps (minute-level boundaries), switch to Swiss Ephemeris (see below).
- Mean node is used for Rahu/Ketu. If you prefer true node, you can swap the node computation.

Quick start (local)
- Option A: Just open index.html in a modern browser.
- Option B: Serve locally for best cross-origin behavior:
  - Python: python -m http.server 8080
  - Node: npx serve
  - Open http://localhost:8080

Deploy to GitHub Pages
1) Create a new GitHub repository and add index.html (from this project).
2) Commit and push to main branch.
3) Go to Settings → Pages.
4) Build and deployment → Source: Deploy from a branch.
5) Branch: main; Folder: / (root). Save.
6) Wait a minute; your site will be at https://YOUR_USERNAME.github.io/YOUR_REPO/

Usage
- Enter date/time of birth (local time).
- Search and pick birthplace from the dropdown (auto-fills latitude/longitude).
- Verify timezone offset (IST: +5.5).
- Click “ஜாதகம் கணக்கிடு”.
- View D1, D9 charts, dasha table, and panchanga details.

Project structure
- index.html — All UI and logic in one file.
- (Optional) /assets — Add screenshots or icons if you want to enhance the page.

Configuration and customization
- Default timezone: change the value attribute of the tz input in index.html.
- Chart style: South-Indian layout is implemented. To change labels/ordering, edit SIGN_TO_CELL and SIGNS_TA.
- Ayanamsa: The function lahiriAyanamsa(dateUTC) is used. Replace with your preferred ayanamsa or Swiss Ephemeris.
- Rahu/Ketu: meanLunarNodeLongitude() returns mean node; add an option to toggle true node if desired.
- Dasha rows: Change slice(0, 18) to display more/less rows.

Switch to Swiss Ephemeris (optional, high precision)
- Why: Exact matches with pro software; robust sunrise, ayanamsa, and node options.
- How (browser/WASM):
  - Use a WASM build (npm swisseph or a CDN). Host ephemeris files (se*.se1) under /ephe/.
  - Compute UT Julian day and call sidereal mode:
    - swe.swe_set_sid_mode(swe.SIDM_LAHIRI, 0, 0)
    - swe.swe_calc_ut(jdut, swe.SUN | swe.MOON | planets, swe.FLG_SWIEPH | swe.FLG_SIDEREAL)
    - swe.swe_calc_ut(jdut, swe.MEAN_NODE, ...) for Rahu; Ketu = +180°
    - swe.swe_houses_ex(jdut, swe.FLG_SIDEREAL, lat, lon, 'W') to get Ascendant
  - Replace the Astronomy Engine calls and ascendant math with Swiss Ephemeris outputs.

Privacy and API limits
- This app runs fully on the client. No personal data is sent to your server.
- Geocoding uses OpenStreetMap Nominatim directly from the browser:
  - For low traffic, it’s fine. For production/heavy traffic, please respect their usage policy, add attribution, and consider your own endpoint or a paid geocoder.

Troubleshooting
- Place not auto-filling: try another spelling or add country (e.g., “Madurai, India”). You can manually enter lat/lon.
- Wrong timezone or DST: adjust the timezone field manually (e.g., +5.5 for IST).
- Charts look empty: ensure you selected a place from the dropdown so lat/lon are set, then click calculate.
- 429 or geocoding errors: you may be rate-limited by Nominatim; try later or self-host a geocoding service.

Roadmap ideas
- More vargas (D3, D7, D10), planetary aspects, exaltation/debilitation, yogas
- True node option, Navamsa degrees
- Auto timezone lookup via a timezone API (e.g., GeoNames)
- Export chart as image/PDF

Credits
- Astronomy Engine (MIT): https://github.com/cosinekitty/astronomy
- OpenStreetMap Nominatim: https://nominatim.org/
- Tamil labels and logic for D1/D9 and dasha integration: this project

License
- Choose a license for your repository (MIT is commonly used for static web apps).
- If you adopt MIT, add a LICENSE file and include attribution for Astronomy Engine.

Contributing
- Issues and pull requests are welcome. Please include sample birth details and expected output/screenshots when reporting calculation differences.

Screenshots
- Place screenshots in /assets and reference them here:
  - ![D1/D9 Tamil Charts](assets/screenshot-d1-d9.png)
  - ![Dasha Table](assets/screenshot-dasha.png)
