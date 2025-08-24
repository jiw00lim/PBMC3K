1) FUNCTIONS → do something (always followed by parentheses)
print("hello")
sc.pp.log1p(adata)
sc.tl.umap(adata)
sc.pl.umap(adata, color="leiden")

2) ARGUMENTS (PARAMETERS) → options inside a function call (name=value)
sc.pl.umap(adata, color="CST3", size=20, title="UMAP of CST3")
sc.pp.neighbors(adata, n_neighbors=15, n_pcs=50)
sc.tl.leiden(adata, resolution=1.0, random_state=0)

3) OBJECTS & METHODS → functions that belong to an object
adata.write("pbmc3k_processed.h5ad")
adata.obs["cell_type"] = inferred_labels
adata.raw = adata

4) SCANPY MODULES

sc.pp = preprocessing

sc.pp.filter_cells(adata, min_genes=200)
sc.pp.filter_genes(adata, min_cells=3)
sc.pp.normalize_total(adata, target_sum=1e4)
sc.pp.log1p(adata)
sc.pp.highly_variable_genes(adata)
sc.pp.scale(adata, max_value=10)


sc.tl = tools

sc.tl.pca(adata)
sc.tl.neighbors(adata, n_neighbors=15, n_pcs=50)
sc.tl.umap(adata, random_state=0)
sc.tl.leiden(adata, resolution=1.0, random_state=0)
sc.tl.rank_genes_groups(adata, groupby="leiden", method="wilcoxon")


sc.pl = plotting

sc.pl.pca(adata, color="CST3")
sc.pl.umap(adata, color=["leiden", "CST3"], ncols=2, size=20, title=["Clusters","CST3"])
sc.pl.violin(adata, ["n_genes_by_counts","pct_counts_mt"], groupby="leiden")

5) COMMON PLOT ARGUMENTS (sc.pl.*)

color → gene name (adata.var_names) or metadata (adata.obs)

size → point size

ncols → number of panels per row if color is a list

title → custom title(s)

palette → list of colors for categorical variables

legend_loc → "on data" | "right margin" | "none"

layer → choose data layer (e.g., "raw")

use_raw → True/False (use adata.raw for gene expression)

show → True/False (display now or return figure)

save → filename suffix to auto-save plot

6) TYPICAL WORKFLOW
sc.pp.filter_cells(adata, min_genes=200)
sc.pp.filter_genes(adata, min_cells=3)
sc.pp.calculate_qc_metrics(adata, qc_vars=["mt"], inplace=True)

sc.pp.normalize_total(adata, target_sum=1e4)
sc.pp.log1p(adata)

sc.pp.highly_variable_genes(adata)
adata = adata[:, adata.var.highly_variable]
adata.raw = adata.copy()

sc.pp.scale(adata, max_value=10)
sc.tl.pca(adata)
sc.pp.neighbors(adata, n_neighbors=15, n_pcs=50)
sc.tl.umap(adata, random_state=0)
sc.tl.leiden(adata, resolution=1.0, random_state=0)

sc.pl.umap(adata, color="leiden")
sc.tl.rank_genes_groups(adata, groupby="leiden", method="wilcoxon")

adata.write("results/pbmc3k_processed.h5ad")

7) REPRODUCIBILITY TIPS
import numpy as np, random
np.random.seed(0)
random.seed(0)

sc.tl.umap(adata, random_state=0)
sc.tl.leiden(adata, resolution=1.0, random_state=0)


Also: pin versions (scanpy, umap-learn, leidenalg, igraph, sklearn, numpy, scipy, anndata).

QUICK VOCAB

function → sc.tl.umap(...)

argument (keyword) → color="leiden"

object → adata (AnnData dataset)

method → adata.write(...)
