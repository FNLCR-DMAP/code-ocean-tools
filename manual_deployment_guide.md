# Manual Code Ocean Deployment Guide

## Part 1: Verify Your GitHub Repo Structure

### Required Files Checklist

For EACH of your 39 tools, verify these files exist:

```
tool-name/
├── code/
│   └── run.sh              ← REQUIRED (must be executable)
└── CO.codeocean/
    └── app-panel.json      ← REQUIRED for batch mode
```

### Quick Verification Commands

Run these commands in your terminal to check all tools:

```bash
# Clone your repo (if not already done)
git clone https://github.com/FNLCR-DMAP/code-ocean-tools.git
cd code-ocean-tools

# Check which tools have run.sh
echo "=== Tools WITH run.sh ===" 
for dir in */; do
    if [ -f "${dir}code/run.sh" ]; then
        echo "✅ ${dir}"
    fi
done

echo ""
echo "=== Tools MISSING run.sh ===" 
for dir in */; do
    if [ ! -f "${dir}code/run.sh" ]; then
        echo "❌ ${dir}"
    fi
done

# Check which tools have app-panel.json
echo ""
echo "=== Tools WITH app-panel.json ===" 
for dir in */; do
    if [ -f "${dir}CO.codeocean/app-panel.json" ]; then
        echo "✅ ${dir}"
    fi
done

echo ""
echo "=== Tools MISSING app-panel.json ===" 
for dir in */; do
    if [ ! -f "${dir}CO.codeocean/app-panel.json" ]; then
        echo "❌ ${dir}"
    fi
done
```

---

## Part 2: Create Capsules on Code Ocean (One-Time Setup)

### For EACH Tool, Do This:

1. Go to: https://poc-nci.codeocean.io
2. Click **"+ New Capsule"**
3. Select **"Start from Scratch"**
4. Enter name (e.g., `boxplot`)
5. Select **Python** environment (or appropriate language)
6. Click **Create**
7. **IMPORTANT:** Copy the capsule ID from the URL
   - URL looks like: `https://poc-nci.codeocean.io/capsule/3093577/tree`
   - Capsule ID is: `3093577`

### Record Your Capsule IDs

| # | Tool Name | Capsule ID |
|---|-----------|------------|
| 1 | analysis_to_csv | |
| 2 | append_annotation | |
| 3 | append_pin_color_rule | |
| 4 | arcsinh_normalization | |
| 5 | binary_to_categorical_annotation | |
| 6 | boxplot | |
| 7 | calculate_centroid | |
| 8 | combine_annotations | |
| 9 | combine_dataframes | |
| 10 | downsample_cells | |
| 11 | hierarchical_heatmap | |
| 12 | histogram | |
| 13 | interactive_spatial_plot | |
| 14 | load_csv_files | |
| 15 | manual_phenotyping | |
| 16 | nearest_neighbor_calculation | |
| 17 | neighborhood_profile | |
| 18 | normalize_batch | |
| 19 | phenograph_clustering | |
| 20 | post-it-python | |
| 21 | quantile_scaling | |
| 22 | relational_heatmap | |
| 23 | rename_labels | |
| 24 | ripley_l_calculation | |
| 25 | sankey_plot | |
| 26 | select_values | |
| 27 | setup_analysis | |
| 28 | spatial_interaction | |
| 29 | spatial_plot | |
| 30 | subset_analysis | |
| 31 | summarize_annotation_statistics | |
| 32 | summarize_dataframe | |
| 33 | tsne_analysis | |
| 34 | umap_transformation | |
| 35 | umap_tsne_pca_visualization | |
| 36 | utag_clustering | |
| 37 | visualize_nearest_neighbor | |
| 38 | visualize_ripley_l | |
| 39 | z-score_normalization | |

---

## Part 3: Deploy Code to Capsules (Manual Method)

### One-Time Setup (Do Once)

```bash
# 1. Clone your GitHub repo
git clone https://github.com/FNLCR-DMAP/code-ocean-tools.git
cd code-ocean-tools
```

### Deploy ONE Tool (Example: boxplot)

```bash
# 2. Create a temporary directory for this tool
mkdir -p ~/temp-capsule
rm -rf ~/temp-capsule/*

# 3. Copy tool files to temp directory
cp -r boxplot/* ~/temp-capsule/

# 4. Go to temp directory
cd ~/temp-capsule

# 5. Initialize git
git init
git add .
git commit -m "Initial commit"

# 6. Add Code Ocean as remote (replace CAPSULE_ID with actual ID)
git remote add codeocean https://git.codeocean.com/capsule-CAPSULE_ID.git

# 7. Push to Code Ocean
# When prompted for credentials:
#   Username: your_codeocean_api_token
#   Password: (leave blank, just press Enter)
git push -u codeocean main --force

# 8. Clean up
cd ..
rm -rf ~/temp-capsule
```

### Deploy ALL Tools (Loop Script)

Save this as `deploy_all.sh`:

```bash
#!/bin/bash

# Your Code Ocean API token
CO_TOKEN="your_codeocean_api_token_here"

# Tool name to Capsule ID mapping (FILL IN YOUR IDs!)
declare -A CAPSULE_IDS=(
    ["analysis_to_csv"]="FILL_ID"
    ["append_annotation"]="FILL_ID"
    ["append_pin_color_rule"]="FILL_ID"
    ["arcsinh_normalization"]="FILL_ID"
    ["binary_to_categorical_annotation"]="FILL_ID"
    ["boxplot"]="FILL_ID"
    ["calculate_centroid"]="FILL_ID"
    ["combine_annotations"]="FILL_ID"
    ["combine_dataframes"]="FILL_ID"
    ["downsample_cells"]="FILL_ID"
    ["hierarchical_heatmap"]="FILL_ID"
    ["histogram"]="FILL_ID"
    ["interactive_spatial_plot"]="FILL_ID"
    ["load_csv_files"]="FILL_ID"
    ["manual_phenotyping"]="FILL_ID"
    ["nearest_neighbor_calculation"]="FILL_ID"
    ["neighborhood_profile"]="FILL_ID"
    ["normalize_batch"]="FILL_ID"
    ["phenograph_clustering"]="FILL_ID"
    ["post-it-python"]="FILL_ID"
    ["quantile_scaling"]="FILL_ID"
    ["relational_heatmap"]="FILL_ID"
    ["rename_labels"]="FILL_ID"
    ["ripley_l_calculation"]="FILL_ID"
    ["sankey_plot"]="FILL_ID"
    ["select_values"]="FILL_ID"
    ["setup_analysis"]="FILL_ID"
    ["spatial_interaction"]="FILL_ID"
    ["spatial_plot"]="FILL_ID"
    ["subset_analysis"]="FILL_ID"
    ["summarize_annotation_statistics"]="FILL_ID"
    ["summarize_dataframe"]="FILL_ID"
    ["tsne_analysis"]="FILL_ID"
    ["umap_transformation"]="FILL_ID"
    ["umap_tsne_pca_visualization"]="FILL_ID"
    ["utag_clustering"]="FILL_ID"
    ["visualize_nearest_neighbor"]="FILL_ID"
    ["visualize_ripley_l"]="FILL_ID"
    ["z-score_normalization"]="FILL_ID"
)

# Directory containing your tools
REPO_DIR="$HOME/code-ocean-tools"
TEMP_DIR="$HOME/temp-capsule"

# Loop through each tool
for tool in "${!CAPSULE_IDS[@]}"; do
    capsule_id="${CAPSULE_IDS[$tool]}"
    
    # Skip if ID not set
    if [ "$capsule_id" = "FILL_ID" ]; then
        echo "⏭️  Skipping $tool (no capsule ID)"
        continue
    fi
    
    echo ""
    echo "=========================================="
    echo "Deploying: $tool → capsule-$capsule_id"
    echo "=========================================="
    
    # Clean temp directory
    rm -rf "$TEMP_DIR"
    mkdir -p "$TEMP_DIR"
    
    # Copy tool files
    cp -r "$REPO_DIR/$tool/"* "$TEMP_DIR/"
    
    # Go to temp directory
    cd "$TEMP_DIR"
    
    # Initialize git and commit
    git init
    git add .
    git commit -m "Deploy $tool"
    
    # Add Code Ocean remote with token in URL
    git remote add codeocean "https://${CO_TOKEN}@git.codeocean.com/capsule-${capsule_id}.git"
    
    # Push to Code Ocean
    if git push -u codeocean main --force; then
        echo "✅ SUCCESS: $tool deployed"
    else
        echo "❌ FAILED: $tool"
    fi
    
    # Return to original directory
    cd "$REPO_DIR"
done

# Cleanup
rm -rf "$TEMP_DIR"

echo ""
echo "=========================================="
echo "Deployment complete!"
echo "=========================================="
```

### How to Use the Script

```bash
# 1. Make script executable
chmod +x deploy_all.sh

# 2. Edit the script to add your:
#    - Code Ocean API token
#    - Capsule IDs for each tool

# 3. Run it
./deploy_all.sh
```

---

## Part 4: Verify Deployment

After deploying, check each capsule on Code Ocean:

1. Go to `https://poc-nci.codeocean.io`
2. Click on each capsule
3. Verify you see:
   - `code/run.sh`
   - `code/format_values.py` (if applicable)
   - `CO.codeocean/app-panel.json`
4. Click **"Run"** to test

---

## Quick Reference

### Git Credentials for Code Ocean

When git asks for credentials:
```
Username: your_api_token_here
Password: (leave blank, press Enter)
```

### Useful Commands

```bash
# Check if tool has correct structure
ls -la boxplot/code/
ls -la boxplot/CO.codeocean/

# Test git connection to Code Ocean
git ls-remote https://YOUR_TOKEN@git.codeocean.com/capsule-XXXXX.git
```

---

## Recommended Approach

1. **Start with ONE tool** (e.g., boxplot)
2. Create capsule on Code Ocean UI
3. Deploy using manual method
4. Verify it works
5. Then create remaining 38 capsules
6. Use the loop script to deploy all
