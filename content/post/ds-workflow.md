---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Ds Workflow"
subtitle: ""
summary: ""
authors: []
tags: []
categories: []
date: 2020-01-15T21:53:45-05:00
lastmod: 2020-01-15T21:53:45-05:00
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

Notes drawn from [Data Science Workflow post from Medium](https://towardsdatascience.com/the-data-science-workflow-43859db0415)
With focus for application on cuisine classification project 

1) Source data access:

create an s3 bucket and push whole data/raw there using DVC
query using [S3 select feature](https://medium.com/@bobhaffner/aws-s3-select-retrieving-subsets-of-tabular-files-7df36cd2d230) 


2) data processing:
turn source data into form suitable for modeling stage 
feature engineering logic must be maintanable
target datasets are reproducible
pipeline is traceable to source representations 

DVC; describe and execute the computation graph 
a) vc for large files
b) lightweight pipelines w reproducibility built in 
c) versioning and experimentation management on top of git 

store raw data in queryable/indexable form 

3) Modeling 
experiment management tool 
pipeline implementation using DVC 

4) model deployment 

start with a pickle file with the trained model as a file resource 
can save model to s3 and serve it via Lamda
model servers: https://medium.com/@vikati/the-rise-of-the-model-servers-9395522b6c58


5) model monitoring 

6) exploration and reporting 
