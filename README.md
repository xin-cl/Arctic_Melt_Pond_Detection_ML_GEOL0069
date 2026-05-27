<a name="readme-top"></a>

![title_banner](images/title_banner.png)

<h1 align="center">Melt Pond Detection with Machine Learning</h1>

<p align="center">
Using k-means machine learning classification to analyse SENTINEL-2 satellite data for arctic melt pond detection.
</p>

<br />

<details>
<summary>Table of Contents</summary>

<ol>
<li>
<a href="#about-the-project">About The Project</a>
<ul>
<li><a href="#background">Background</a></li>
<li><a href="#the-sentinel-2-satellite">The SENTINEL-2 Satellite</a></li>
<li><a href="#machine-learning-methodology-k-means-clustering">Machine Learning Methodology: K-Means Clustering</a></li>
</ul>
</li>

<li>
<a href="#getting-started">Getting Started</a>
<ul>
<li><a href="#datasets-used">Datasets Used</a></li>
</ul>
</li>

<li><a href="#usage">Usage</a></li>
<li><a href="#license">License</a></li>
<li><a href="#contact">Contact</a></li>
<li><a href="#acknowledgments">Acknowledgments</a></li>
<ul>
<li><a href="#references">References</a></li>
</ul>

</ol>
</details>


# About The Project

This project was created as a final assignment for GEOL0069 at University College London to explore the use of machine learning (ML) algorithms within Earth Science applications. It utilises unsupervised learning methods to detect and extract melt ponds from satellite imagery. SENTINEL-2 imagery was selected due to its high spatial resolution and multi-spectral capabilities, which are suitable for distinguishing meltwater from snow and ice surfaces. The primary algorithm implemented throughout this project is K-means clustering.

## Background

Melt ponds are pools of liquid water that form on the surface of sea ice and glaciers during seasonal melting. They significantly alter the surface albedo of ice-covered regions and can accelerate melting processes by absorbing greater amounts of incoming solar radiation.

Accurate identification and monitoring of melt ponds are important because they influence Arctic climate feedback systems, sea ice dynamics, and long-term climate modelling. Traditional field measurements are spatially limited and difficult to conduct in remote polar regions. Satellite remote sensing therefore provides a scalable alternative for observing melt pond evolution over large geographic areas.

The optical properties of melt ponds differ from surrounding snow and ice surfaces, allowing multispectral satellite imagery to distinguish them. However, variability in pond depth, ice conditions, cloud contamination, and shadow effects can make classification difficult.

This project investigates whether machine learning techniques can improve melt pond identification using SENTINEL-2 imagery.

### Normalized Difference Water Index (NDWI)

One commonly used approach for identifying water features is the Normalized Difference Water Index (NDWI), proposed by McFeeters (1996):

$$
\text{NDWI}=
\frac{\text{Green}-\text{NIR}}
{\text{Green}+\text{NIR}}
$$

where:

- Green = green wavelength band  
- NIR = near infrared wavelength band

The index enhances water features while suppressing vegetation and land surfaces.

While NDWI performs effectively for many open-water applications, melt ponds present additional challenges. Snow, bare ice, slush, and shallow ponds can possess similar spectral characteristics, reducing NDWI performance. Consequently, NDWI serves as a baseline comparison against machine learning approaches in this study.

The goal of this project is to determine whether ML methods can provide improved discrimination between melt ponds and surrounding cryospheric surfaces.

<p align="right">(<a href="#readme-top">back to top</a>)</p>


# The SENTINEL-2 Satellite

SENTINEL-2 is a European multispectral Earth observation mission developed under the Copernicus Programme. The mission supports land monitoring, environmental observation, disaster management, and climate studies.

The mission consists of SENTINEL-2A and SENTINEL-2B satellites operating in the same orbit and separated by 180°. Together they provide global coverage with frequent revisit times.

Both satellites carry a Multi-Spectral Instrument (MSI) that captures imagery across 13 spectral bands:

- Four bands at 10 m resolution
- Six bands at 20 m resolution
- Three bands at 60 m resolution

These spectral bands enable identification of land cover and cryospheric features based on their unique reflectance signatures.

For melt pond detection, useful bands include:

| Band | Description | Resolution |
|---|---:|---|
| B2 | Blue | 10m |
| B3 | Green | 10m |
| B4 | Red | 10m |
| B8 | Near Infrared | 10m |
| B11 | SWIR | 20m |

Differences in reflectance between water, snow, and ice across these wavelengths form the basis for classification.

### Multi-Spectral Instrument (MSI)

The MSI operates using a push-broom imaging system. Rather than capturing a full image at once, the instrument continuously scans strips of Earth as the satellite moves along its orbit.

Incoming reflected radiation is separated into different wavelength bands using optical filters and detector arrays. This enables simultaneous acquisition of visible, near-infrared, and shortwave infrared information.

The spectral diversity of MSI data makes it particularly useful for distinguishing melt ponds from adjacent ice surfaces.

![how_sen2_works](images/sentinel2_diagram.png)

<p align="right">(<a href="#readme-top">back to top</a>)</p>


# Machine Learning Methodology: K-Means Clustering

The algorithm used throughout this project is K-means clustering.

K-means partitions a dataset into *k* groups by assigning observations to clusters according to spectral similarity.

The algorithm begins by defining *k* cluster centres (centroids). Pixels are then assigned to the nearest centroid according to Euclidean distance. Cluster positions are updated iteratively until convergence.

Because melt ponds possess spectral characteristics distinct from snow and ice, clustering methods can potentially separate these surfaces without requiring manually labelled training data.

K-means is well suited to exploratory remote sensing applications because:

- no prior labels are required
- spectral structures emerge directly from data
- it can rapidly process large image datasets

![how_kmeans_works](images/kmeans_diagram.png)

### Key Components of K-Means

1. **Choosing K**  
Number of desired clusters specified beforehand.

2. **Centroid Initialisation**  
Initial placement affects convergence and results.

3. **Assignment**  
Pixels assigned according to Euclidean distance.

4. **Update**  
Cluster centres recalculated.

5. **Iterate**  
Repeated until centroids stabilize.

<p align="right">(<a href="#readme-top">back to top</a>)</p>


# Getting Started

This project was developed using Google Colab.

To get started:

1. Download `ML_Melt_Pond_Detection.ipynb`
2. Download the required datasets
3. Modify file paths within the notebook
4. Run notebook cells sequentially

Further instructions are provided within the notebook.


## Datasets Used

The datasets used in this project are SENTINEL-2 Level-2B products.

Study areas focus on Arctic and Greenland regions where seasonal melt pond formation is prevalent.

Potential study sites include:

- Greenland Ice Sheet
- Canadian Arctic Archipelago
- Beaufort Sea
- Fram Strait sea ice

These regions were selected because they exhibit:

- seasonal melt pond formation
- varying pond densities
- complex ice conditions
- diverse spectral characteristics

### Downloading Datasets

Datasets are not included due to file size limitations.

Data can be downloaded from the Copernicus Dataspace Browser:

https://browser.dataspace.copernicus.eu/

Search for SENTINEL-2 Level-2A scenes over Arctic regions during summer melt seasons (June–August).

Recommended filters:

- Product type: L2A
- Cloud cover: <10%
- Season: peak summer melt
- Region: Greenland / Arctic sea ice

Download and extract `.SAFE` products before use.

<p align="right">(<a href="#readme-top">back to top</a>)</p>


# Usage

This notebook performs:

- preprocessing of Sentinel-2 imagery
- spectral band extraction
- optional index calculation
- K-means clustering
- classification visualisation
- comparison against NDWI baseline methods

Outputs include classified melt pond masks and visual comparisons.


<p align="right">(<a href="#readme-top">back to top</a>)</p>


# License

Distributed under the MIT License.

See `LICENSE.txt` for more information.

<p align="right">(<a href="#readme-top">back to top</a>)</p>


# Contact

Your Name

xincl2634@gmail.com

Project Link:

https://github.com/yourusername/GEOL0069-ML-Melt-Pond-Detection](https://github.com/xin-cl/Arctic_Melt_Pond_Detection_ML_GEOL0069

<p align="right">(<a href="#readme-top">back to top</a>)</p>



# Acknowledgments

This project was created for GEOL0069 at University College London.

## References

McFeeters, S.K. (1996). The use of the Normalized Difference Water Index in the delineation of open water features.

Perovich, D.K., et al. Melt pond evolution on Arctic sea ice.

Sentinel-2 Mission Documentation.

GEOL0069 Guidebook — Machine Learning and Earth Observation.

<p align="right">(<a href="#readme-top">back to top</a>)</p>
