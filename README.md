# Voronoi tessellation for tissue architecture quantification in H&E

Part of the **Digital Pathology** playlist on the [DigitalSreeni YouTube channel](https://www.youtube.com/@DigitalSreeni).

This tutorial uses Voronoi tessellation to partition H&E tissue into exclusive cell territories and quantify tissue architecture from the resulting region geometry. Each cell owns the space closer to it than to any other cell. Coloring those regions by cell type gives an immediate visual partition of the tissue into tumor, immune, and stromal zones — no explicit segmentation, no deep learning, no special staining.

This is the fourth video in the spatial analysis mini-series. The previous three used the radial distribution function, Ripley's K function, and Delaunay triangulation. Voronoi tessellation is the mathematical dual of Delaunay triangulation — the two structures encode the same spatial adjacency from complementary perspectives.

---

## What this notebook covers

- Nucleus segmentation with Cellpose (reused from previous tutorials)
- Three-class cell classification: lymphocyte, tumor cell, stromal
- Building a Voronoi tessellation with `scipy.spatial.Voronoi`
- Handling infinite boundary regions using mirror points
- Clipping regions to the image frame using shapely
- Visualizing the tessellation colored by cell type
- Computing region area and compactness per cell
- Mapping region area spatially as a packing density map
- Verifying Voronoi neighbor counts match Delaunay neighbor counts

---

## Files

| File | Description |
|---|---|
| `voronoi_tissue_architecture.ipynb` | Main tutorial notebook |
| `Tumor_islands_in_fibrous_stroma.png` | H&E image used in the tutorial |

---

## H&E image

The H&E image was generated using ChatGPT with the following prompt:

> Generate a realistic H&E histology image of invasive squamous cell carcinoma. Show two to three irregular tumor nests with large pleomorphic cells, prominent nucleoli, and intercellular bridges. Surround the nests with a dense rim of small dark lymphocytes forming a clear excluded immune phenotype. Include moderate lymphocyte presence in the peri-tumoral fibrous stroma and sparse lymphocytes in the distal stroma. Add a few lymphocytes inside the nests as tumor-infiltrating lymphocytes. Include fibrous stromal bands between nests. Add realistic H&E artifacts including slight color variation, occasional red blood cells, and tissue folds. The overall color balance should be hematoxylin-dominant with a pink eosin background.

---

## Requirements
numpy
Pillow
cellpose>=4.0
scikit-image
scipy
matplotlib
shapely

Install dependencies:

```bash
pip install cellpose shapely
```

---

## Key results on the tutorial image

| Metric | Value |
|---|---|
| Total nuclei detected | 3,997 |
| Lymphocytes | 2,591 |
| Tumor cells | 531 |
| Stromal / unclassified | 875 |
| Valid Voronoi regions | 3,997 |
| Median region area — lymphocytes | 132 px |
| Median region area — tumor cells | 563 px |
| Median region area — stromal | 606 px |
| Median compactness — lymphocytes | 0.775 |
| Median compactness — tumor cells | 0.809 |
| Median compactness — stromal | 0.758 |
| Median Voronoi neighbor count (all types) | 6 |

---

## The mathematical duality

Voronoi tessellation and Delaunay triangulation are mathematical duals. Every Delaunay edge corresponds to a shared Voronoi boundary and vice versa. Voronoi neighbor counts equal Delaunay neighbor counts exactly. The notebook verifies this as a sanity check.

---

## Related tutorials in this series

| Video | Method | Question |
|---|---|---|
| Spatial immune analysis with RDF | Radial distribution function | Where are lymphocytes relative to the tumor boundary? |
| TLS detection with Ripley's K | Ripley's K function | Are lymphocytes globally clustered? |
| Cell neighborhood analysis | Delaunay triangulation | What cell types surround each individual cell? |
| This video | Voronoi tessellation | How much space does each cell own and how regular is it? |

---

## Key references

- Okabe et al. (2000) Spatial Tessellations: Concepts and Applications of Voronoi Diagrams. Wiley.
- Schurch et al. (2020) Nature. Coordinated cellular neighborhoods orchestrate antitumoral immunity at the colorectal cancer invasive front.
- Saltz et al. (2018) Cell Reports. Spatial organization and molecular correlation of tumor-infiltrating lymphocytes using deep learning on pathology images.

---

[YouTube channel](https://www.youtube.com/@DigitalSreeni) | [GitHub](https://github.com/bnsreenu/digitalpathology-voronoi-tessellation)