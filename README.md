# README.md

> Group Member: Xue LIAO, Linjie QIU, Yihang LIU, Zhe DING

## Workflow for 3D Gaussian Splatting Dataset Management

### 1. Prerequisites
#### For All Users
- Install [Git](https://git-scm.com/)
- Install [Git LFS](https://git-lfs.com/)
- Request repository access from project admin

```bash
# One-time LFS initialization
git lfs install
```

---

### 2. For Artists (Non-Technical Workflow)
#### A. First-Time Setup
1. Install [GitHub Desktop](https://desktop.github.com/)
2. Clone repository: File > Clone Repository > Select project
3. Install [Git LFS Client](https://git-lfs.com/)

#### B. Daily Operations
**Add New Assets**

1. Place new files in designated folders:
   ```
   /assets/point_clouds/*.ply
   /assets/textures/*.bin
   ```
2. In GitHub Desktop:

   - ✔️ Select changed files
   - ✏️ Write commit message (e.g., "Add scan_0234 assets")
   - Click "Push origin"


**Update from Others**

1. Click "Fetch origin"
2. Click "Pull origin"

**Verify LFS Tracking**

Check file status in repository > "Git LFS" tab on GitHub.com

---

### 3. For Engineers (CLI Workflow)
#### A. Repository Setup
```bash
git clone https://github.com/yourorg/3d-gaussian-dataset.git
cd 3d-gaussian-dataset
git lfs pull
```

#### B. File Operations
**Add/Modify Large Files**

```bash
# Stage LFS-tracked files
git add assets/splat_data/gaussian_0234.pt

# Commit and push
git commit -m "Update gaussian splatting model"
git push
```

**Partial Dataset Download**

```bash
git lfs pull --include="assets/textures"
```

**Migration of Existing Files**

```bash
git lfs migrate import --include="*.ply,*.bin,*.pt" --everything
```

---

### 4. File Structure Conventions
```
├── assets/
│   ├── point_clouds/      # .ply files (Gaussian positions)
│   ├── textures/          # .bin files (compressed textures)
│   └── splat_data/        # .pt files (PyTorch splatting models)
├── scripts/
│   ├── preprocessing.py   # Dataset processing
│   └── visualization.ipynb
└── .gitattributes          # LFS tracking rules
```

---

### 5. Git LFS Configuration
**Tracked File Types (in `.gitattributes`)**

```
*.ply filter=lfs diff=lfs merge=lfs -text
*.bin filter=lfs diff=lfs merge=lfs -text
*.pt filter=lfs diff=lfs merge=lfs -text
assets/**/*.h5 filter=lfs diff=lfs merge=lfs -text
```

**Verify Tracking Status**

```bash
git lfs ls-files
```

---

### 6. Best Practices
1. **File Size Limits**

   - Keep individual files <5GB

   - Split large datasets into chunks

2. **Avoid in .gitignore**
   ```plaintext
   *.tmp
   __pycache__/
   ```

3. **Troubleshooting**
```bash
# Recover missing LFS files
git lfs fetch --all
git lfs checkout

# Force LFS push
git lfs push origin --all
```

---

### 7. Dataset Versioning Strategy
| Version Tag      | Description                   |
| ---------------- | ----------------------------- |
| `v1.0-base`      | Initial splatting dataset     |
| `v1.1-textures`  | Updated texture maps          |
| `v2.0-optimized` | Quantized models & compressed |

