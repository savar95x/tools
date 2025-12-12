name: Deploy Tools

on:
  push:
    branches: ["main"]

permissions:
  contents: read
  pages: write
  id-token: write

jobs:
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: 20

      # --------------------------------------------------------

      - name: Build Breathe
        run: |
          cd breathe
          npm ci
          npm run build

      - name: Build Pss Website
        run: |
          cd pss-website
          npm ci
          npm run build 

      - name: Build Family Nexus
        run: |
          cd family-nexus
          npm ci
          npm run build 

      # MERGE BUILDS
      # --------------------------------------------------------
      - name: Prepare Deployment Folder
        run: |
          mkdir _site
          # Copy the main landing page if you have one
          #cp index.html _site/ 
          
          # Move app builds into their subfolders in _site
          mkdir -p _site/breathe
          cp -r breathe/dist/* _site/breathe/
          
          mkdir -p _site/pss-website
          cp -r pss-website/dist/* _site/pss-website/

          mkdir -p _site/family-nexus
          cp -r family-nexus/dist/* _site/family-nexus/

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: '_site'

      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
