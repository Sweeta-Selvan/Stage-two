Summary of scRNA sequencing pipeline for bone marrow dataset
step1 : Installed the libraries like scanpy, igraph, decoupler, celltypist in my colab notebook
and imported the libraries.
step 2 : Get the link of dataset file and fetch it in colab and then read the file.
step 3 : Explore the rows and columns of the dataset.
step 4 : Identified the stressed cells by looking at Mitochondrial gene percentage and ribosomal contamination, Haemoglobin contamination.
step 5 : Then  QC metrics, i.e., the percent of MT, RIBO, HB present is calculated
step 6 : After finding QC metrics, violin plot is plotted for each of the above genes
step 7 : In violin plot for "pct_counts_MT" - (percent of MT genes), nearly all the cells are distributed under 5% (threshold for MTgenes) and RIBO genes are also under thresold (<10) and HB also under threshold.
The violin plot basically shows the distribution of data (here QC metrics)
step 8 : Then we plotted scatter plot for three , n_genes_by_counts, total_counts, "pct_counts_MT" to show how trend changes and relationship between the metrics and will help to determine no of genes
and cells to be filtered. Same repeated for RIBO and HB metrics.
step 9 : DOublet cells detection also performed, if there is more doublet cell it needs to be removed.
step 10 : Then Normalization of data is done to scale down values in same range, so they become comparable and to remove technical bias and log transformed to stabilize the variance.
step 11 : Picked the higly variable genes among the cells, the one that has varying expression among cells and plotted it. 
step 12 : Dimensionality reduction is done to reduce the complexity of the data, so the important and crucial features are picked so comparision becomes easier. Principal compenet analysis (PCA) was done to 
pick first 10 PCs and then plotted it to find the variance ratio between PCs. It was found the first six PCs ahve significant variance, rest do not have.
step 13 : Then PCA plot of "pct_counts_MT" genes is plotted to if the variation is driven by MT% , the plot did not show such variation(same repeated for RIBO, HB genes). 
PCA performs linear dimensionality reduction
step 14 : The neighbor graph calculation is done which is essential for UMAP. UMAP performs non-linear dimensionality reduction and maps the cell in 2D.
step 15 : Then Leiden clustering is performed which showed 7-8 clusters, may belong to different cell type.
step 16 : Then leiden clustering is performed at different resolutions like 0.02, 0.5, 2.at resoltion 0.02 large and fewer clusters are there. With increasing resolutions to 2, number of clusters increased t0 38.
step 17 : Leiden clustering at different resolutions is plotted using umap
step 18 : Using decoupler, the marker genes are imported and compared with genes of our cluster.
step 19 : after comparision, the clusters are biologically defined by ranking genes.
step 20 : Finally, Clusters are annotated as distinct cell types.

The cluster annotations are
 Cell Type                 Clusters  
 Neutrophils              | 0        
 Monocytes                | 1        
 NK cells                 | 2, 3     
 Nuocytes (ILC2)          | 4, 7     
 Naive B cells            | 5        
 Platelets                | 6        
 Gamma delta T cells      | 8, 10    
 Plasma cells             | 9        
 Hematopoietic stem cells | 11       




#Neutrophils -Fast first-responders that kill microbes using phagocytosis, ROS, and granule enzymes.

#Monocytes -Circulate in blood and become macrophages/dendritic cells to clear pathogens and drive inflammation.

#NK cells -Kill virus-infected and tumor cells using perforin–granzyme and produce IFN-γ to boost immunity.

#Nuocytes (ILC2) -Produce IL-4/IL-5/IL-13 to drive type-2 immunity, allergies, and tissue repair.

#Naive B cells -Inactive B cells that, upon antigen exposure, become plasma or memory B cells.

#Platelets -Drive blood clotting and release factors for wound healing and inflammation.

#Gamma delta T cells -Innate-like T cells that detect stressed cells and rapidly release cytokines like IFN-γ/IL-17.

#Plasma cells -Antibody-secreting cells that provide strong, long-term humoral immunity.

#Hematopoietic Stem Cells -Self-renewing stem cells that generate all blood and immune cell lineages

=================================================================================================================================================================================================================
Expected vs missing lineage populations
=

-Erythroid lineage clusters (proerythroblast → basophilic → polychromatophilic) with high expression of HBB, HBA1, HBA2, ALAS2, GYPA.
-Megakaryocyte / megakaryocyte-progenitor signatures (e.g., PF4, ITGA2B (CD41), GP1BA, VWF and transcriptional programs with GATA1).
-Myeloid progenitors (CMP/GMP and immature granulocyte stages) showing stepwise expression of MPO, ELANE, AZU1, CSF3R → mature neutrophil markers.
-Early B-cell developmental stages (pro-B / pre-B) expressing RAG1/2, DNTT, VPREB1 in addition to CD19/CD79A.
-A progenitor-rich structure: HSCs + CMP/GMP + MEP + lineage-committed intermediates forming differentiation trajectories.

-What dataset shows :
-Annotated clusters: neutrophils, monocytes, NK cells, nuocytes, naive B cells, plasma cells, platelets, gamma-delta T cells — i.e., mature circulating immune cells.
-No reported erythroid clusters and no clear megakaryocyte progenitor clusters (platelets present but not equivalent to megakaryocyte progenitors).
-Only one small HSC cluster (cluster 11); 
Conclusion : The absence of erythroid and megakaryocytic progenitors (two of the most abundant and characteristic marrow populations) is a major biological mismatch with bone marrow. 
This alone strongly favors peripheral blood.

typical frequency distributions
=
Bone marrow (typical adult aspirate):
-Large fractions of erythroid precursors (often ≥20% of nucleated cells); myeloid precursors also sizeable.
-HSCs/CD34+ cells are rare (~0.01–0.1% without mobilization).
-Mature circulating lymphocytes are a smaller proportion relative to progenitors.
Peripheral blood / PBMC:
-Dominated by mature neutrophils, lymphocytes (B, T, NK), monocytes; erythroid precursors are absent.
-Platelets abundant as fragments. HSCs essentially absent (unless mobilized/enriched).
sample (qualitative frequency evidence):
-UMAP + cluster map show large mature immune clusters (neutrophils, NK/T, naive B) and only one small HSC cluster.
-HSC violin shows cluster 11 high score but many mature clusters have moderate HSC score “bleed” .
Therefore the composition and relative cluster sizes align with peripheral blood frequency distributions, not marrow.
Conclusion: The observed frequencies (many mature cells, negligible progenitor compartments) are inconsistent with marrow and consistent with PB/ PBMC.

presence or absence of progenitors
=
True bone marrow should contain multiple progenitor compartments, including:

Hematopoietic stem cells (HSC),Common myeloid progenitors (CMP), Granulocyte–monocyte progenitors (GMP), Megakaryocyte–erythroid progenitors (MEP),Early B-cell precursors (Pre-pro-B, Pro-B, Pre-B),
Dendritic-cell progenitors.
But in our clusters, all mature or end-stage differentiated populations of cells are present.In peripheral blood, a small number of circulating HSCs are normal.
Without downstream progenitor intermediates, a lone HSC cluster does not represent bone marrow.
True HSCs in marrow appear with adjacent early progenitors, which are absent here.
So this HSC cluster is most likely circulating stem cells, not marrow-resident progenitors.
Conclusion: 
Because:Progenitor lineages are absent,
Only mature lymphoid and myeloid effector cells dominate,
There is enrichment of mature lymphocytes, and
No differentiation gradients exist,the sample is not bone marrow. The absence of progenitors biologically favors a peripheral blood / PBMC source.

Healthy or infected
=
nuocytes, b cells naive, gamma delta T cells , neutrophils are top  four abundant cell clusters, indicating the sample is from infected  patient. Presence of these cells indicates allergic environment.
neutrophils > monocytes with NK cells comparable to neutrophils — points to an innate -inflammation.The lack of a large monocyte expansion and absence of a clear activated-NK transcriptional signature 
(cytotoxic genes) would favour mild-to-moderate inflammation or contamination.
