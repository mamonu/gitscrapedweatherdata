# scrapeddataanalysis




got some data.

had the following .github/workflows/scrape.yml


```

name: Scrape latest data

on:
  push:
  workflow_dispatch:
  schedule:
  - cron: '14 * * * *'

jobs:
  scheduled:
    runs-on: ubuntu-latest
    steps:
    - name: Check out this repo
      uses: actions/checkout@v3
      with:
        token: ${{ secrets.GITHUB_TOKEN }}
    - name: Fetch latest data
      run: |-
        timestamp=$(date -u +"%Y%m%d%H%M%S")
        curl "https://api.open-meteo.com/v1/forecast?latitude=51.33&longitude=0.26&current_weather=true" | jq >> weather_in_EpsomUK.json
        curl "https://api.open-meteo.com/v1/forecast?latitude=38.8997&longitude=22.4337&current_weather=true" | jq >> weather_in_LamiaGR.json
    - name: Commit and push if it changed
      run: |-
        git config user.name "Automated"
        git config user.email "actions@users.noreply.github.com"
        git add -A
        git commit -m "Latest data: ${timestamp}" || exit 0
        git push


```
