cola Report for Consensus Partitioning
==================

**Date**: 2019-12-02 22:18:30 CET, **cola version**: 1.3.2

----------------------------------------------------------------

<style type='text/css'>

body, td, th {
   font-family: Arial,Helvetica,sans-serif;
   background-color: white;
   font-size: 13px;
  max-width: 800px;
  margin: auto;
  margin-left:210px;
  padding: 0px 10px 0px 10px;
  border-left: 1px solid #EEEEEE;
  line-height: 150%;
}

tt, code, pre {
   font-family: 'DejaVu Sans Mono', 'Droid Sans Mono', 'Lucida Console', Consolas, Monaco, 

monospace;
}

h1 {
   font-size:2.2em;
}

h2 {
   font-size:1.8em;
}

h3 {
   font-size:1.4em;
}

h4 {
   font-size:1.0em;
}

h5 {
   font-size:0.9em;
}

h6 {
   font-size:0.8em;
}

a {
  text-decoration: none;
  color: #0366d6;
}

a:hover {
  text-decoration: underline;
}

a:visited {
   color: #0366d6;
}

pre, img {
  max-width: 100%;
}
pre {
  overflow-x: auto;
}
pre code {
   display: block; padding: 0.5em;
}

code {
  font-size: 92%;
  border: 1px solid #ccc;
}

code[class] {
  background-color: #F8F8F8;
}

table, td, th {
  border: 1px solid #ccc;
}

blockquote {
   color:#666666;
   margin:0;
   padding-left: 1em;
   border-left: 0.5em #EEE solid;
}

hr {
   height: 0px;
   border-bottom: none;
   border-top-width: thin;
   border-top-style: dotted;
   border-top-color: #999999;
}

@media print {
   * {
      background: transparent !important;
      color: black !important;
      filter:none !important;
      -ms-filter: none !important;
   }

   body {
      font-size:12pt;
      max-width:100%;
   }

   a, a:visited {
      text-decoration: underline;
   }

   hr {
      visibility: hidden;
      page-break-before: always;
   }

   pre, blockquote {
      padding-right: 1em;
      page-break-inside: avoid;
   }

   tr, img {
      page-break-inside: avoid;
   }

   img {
      max-width: 100% !important;
   }

   @page :left {
      margin: 15mm 20mm 15mm 10mm;
   }

   @page :right {
      margin: 15mm 10mm 15mm 20mm;
   }

   p, h2, h3 {
      orphans: 3; widows: 3;
   }

   h2, h3 {
      page-break-after: avoid;
   }
}
</style>




## Summary





All available functions which can be applied to this `res_list` object:


```r
res_list
```

```
#> A 'ConsensusPartitionList' object with 24 methods.
#>   On a matrix with 21168 rows and 54 columns.
#>   Top rows are extracted by 'SD, CV, MAD, ATC' methods.
#>   Subgroups are detected by 'hclust, kmeans, skmeans, pam, mclust, NMF' method.
#>   Number of partitions are tried for k = 2, 3, 4, 5, 6.
#>   Performed in total 30000 partitions by row resampling.
#> 
#> Following methods can be applied to this 'ConsensusPartitionList' object:
#>  [1] "cola_report"           "collect_classes"       "collect_plots"         "collect_stats"        
#>  [5] "colnames"              "functional_enrichment" "get_anno_col"          "get_anno"             
#>  [9] "get_classes"           "get_matrix"            "get_membership"        "get_stats"            
#> [13] "is_best_k"             "is_stable_k"           "ncol"                  "nrow"                 
#> [17] "rownames"              "show"                  "suggest_best_k"        "test_to_known_factors"
#> [21] "top_rows_heatmap"      "top_rows_overlap"     
#> 
#> You can get result for a single method by, e.g. object["SD", "hclust"] or object["SD:hclust"]
#> or a subset of methods by object[c("SD", "CV")], c("hclust", "kmeans")]
```

The call of `run_all_consensus_partition_methods()` was:


```
#> run_all_consensus_partition_methods(data = mat, mc.cores = 4, anno = anno)
```

Dimension of the input matrix:


```r
mat = get_matrix(res_list)
dim(mat)
```

```
#> [1] 21168    54
```

### Density distribution

The density distribution for each sample is visualized as in one column in the
following heatmap. The clustering is based on the distance which is the
Kolmogorov-Smirnov statistic between two distributions.




```r
library(ComplexHeatmap)
densityHeatmap(mat, top_annotation = HeatmapAnnotation(df = get_anno(res_list), 
    col = get_anno_col(res_list)), ylab = "value", cluster_columns = TRUE, show_column_names = FALSE,
    mc.cores = 4)
```

![plot of chunk density-heatmap](figure_cola/density-heatmap-1.png)





### Suggest the best k



Folowing table shows the best `k` (number of partitions) for each combination
of top-value methods and partition methods. Clicking on the method name in
the table goes to the section for a single combination of methods.

[The cola vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13)
explains the definition of the metrics used for determining the best
number of partitions.


```r
suggest_best_k(res_list)
```


|                            | The best k| 1-PAC| Mean silhouette| Concordance|   |Optional k |
|:---------------------------|----------:|-----:|---------------:|-----------:|:--|:----------|
|[SD:hclust](#SD-hclust)     |          2| 1.000|           0.980|       0.989|** |           |
|[CV:pam](#CV-pam)           |          2| 1.000|           0.962|       0.984|** |           |
|[MAD:mclust](#MAD-mclust)   |          3| 1.000|           0.973|       0.988|** |           |
|[ATC:hclust](#ATC-hclust)   |          2| 1.000|           0.978|       0.987|** |           |
|[ATC:kmeans](#ATC-kmeans)   |          2| 1.000|           0.997|       0.994|** |           |
|[ATC:skmeans](#ATC-skmeans) |          2| 1.000|           0.974|       0.991|** |           |
|[ATC:pam](#ATC-pam)         |          2| 1.000|           0.999|       0.999|** |           |
|[ATC:mclust](#ATC-mclust)   |          2| 1.000|           0.977|       0.989|** |           |
|[CV:hclust](#CV-hclust)     |          4| 0.991|           0.927|       0.973|** |           |
|[SD:mclust](#SD-mclust)     |          3| 0.978|           0.929|       0.963|** |2          |
|[MAD:hclust](#MAD-hclust)   |          4| 0.972|           0.916|       0.969|** |2,3        |
|[ATC:NMF](#ATC-NMF)         |          3| 0.968|           0.961|       0.976|** |2          |
|[CV:mclust](#CV-mclust)     |          3| 0.957|           0.947|       0.971|** |           |
|[MAD:pam](#MAD-pam)         |          5| 0.944|           0.891|       0.957|*  |2          |
|[SD:NMF](#SD-NMF)           |          3| 0.944|           0.924|       0.967|*  |2          |
|[SD:skmeans](#SD-skmeans)   |          2| 0.925|           0.947|       0.977|*  |           |
|[MAD:kmeans](#MAD-kmeans)   |          5| 0.919|           0.950|       0.929|*  |           |
|[CV:NMF](#CV-NMF)           |          2| 0.887|           0.937|       0.972|   |           |
|[MAD:skmeans](#MAD-skmeans) |          3| 0.887|           0.892|       0.952|   |           |
|[MAD:NMF](#MAD-NMF)         |          2| 0.885|           0.920|       0.967|   |           |
|[CV:kmeans](#CV-kmeans)     |          3| 0.869|           0.937|       0.946|   |           |
|[SD:kmeans](#SD-kmeans)     |          3| 0.859|           0.961|       0.968|   |           |
|[SD:pam](#SD-pam)           |          3| 0.806|           0.916|       0.943|   |           |
|[CV:skmeans](#CV-skmeans)   |          2| 0.742|           0.923|       0.963|   |           |

\*\*: 1-PAC > 0.95, \*: 1-PAC > 0.9




### CDF of consensus matrices

Cumulative distribution function curves of consensus matrix for all methods.




```r
collect_plots(res_list, fun = plot_ecdf)
```

![plot of chunk collect-plots](figure_cola/collect-plots-1.png)



### Consensus heatmap

Consensus heatmaps for all methods. ([What is a consensus heatmap?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_9))


<style type='text/css'>



.ui-helper-hidden {
	display: none;
}
.ui-helper-hidden-accessible {
	border: 0;
	clip: rect(0 0 0 0);
	height: 1px;
	margin: -1px;
	overflow: hidden;
	padding: 0;
	position: absolute;
	width: 1px;
}
.ui-helper-reset {
	margin: 0;
	padding: 0;
	border: 0;
	outline: 0;
	line-height: 1.3;
	text-decoration: none;
	font-size: 100%;
	list-style: none;
}
.ui-helper-clearfix:before,
.ui-helper-clearfix:after {
	content: "";
	display: table;
	border-collapse: collapse;
}
.ui-helper-clearfix:after {
	clear: both;
}
.ui-helper-zfix {
	width: 100%;
	height: 100%;
	top: 0;
	left: 0;
	position: absolute;
	opacity: 0;
	filter:Alpha(Opacity=0); 
}

.ui-front {
	z-index: 100;
}



.ui-state-disabled {
	cursor: default !important;
	pointer-events: none;
}



.ui-icon {
	display: inline-block;
	vertical-align: middle;
	margin-top: -.25em;
	position: relative;
	text-indent: -99999px;
	overflow: hidden;
	background-repeat: no-repeat;
}

.ui-widget-icon-block {
	left: 50%;
	margin-left: -8px;
	display: block;
}




.ui-widget-overlay {
	position: fixed;
	top: 0;
	left: 0;
	width: 100%;
	height: 100%;
}
.ui-accordion .ui-accordion-header {
	display: block;
	cursor: pointer;
	position: relative;
	margin: 2px 0 0 0;
	padding: .5em .5em .5em .7em;
	font-size: 100%;
}
.ui-accordion .ui-accordion-content {
	padding: 1em 2.2em;
	border-top: 0;
	overflow: auto;
}
.ui-autocomplete {
	position: absolute;
	top: 0;
	left: 0;
	cursor: default;
}
.ui-menu {
	list-style: none;
	padding: 0;
	margin: 0;
	display: block;
	outline: 0;
}
.ui-menu .ui-menu {
	position: absolute;
}
.ui-menu .ui-menu-item {
	margin: 0;
	cursor: pointer;
	
	list-style-image: url("data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7");
}
.ui-menu .ui-menu-item-wrapper {
	position: relative;
	padding: 3px 1em 3px .4em;
}
.ui-menu .ui-menu-divider {
	margin: 5px 0;
	height: 0;
	font-size: 0;
	line-height: 0;
	border-width: 1px 0 0 0;
}
.ui-menu .ui-state-focus,
.ui-menu .ui-state-active {
	margin: -1px;
}


.ui-menu-icons {
	position: relative;
}
.ui-menu-icons .ui-menu-item-wrapper {
	padding-left: 2em;
}


.ui-menu .ui-icon {
	position: absolute;
	top: 0;
	bottom: 0;
	left: .2em;
	margin: auto 0;
}


.ui-menu .ui-menu-icon {
	left: auto;
	right: 0;
}
.ui-button {
	padding: .4em 1em;
	display: inline-block;
	position: relative;
	line-height: normal;
	margin-right: .1em;
	cursor: pointer;
	vertical-align: middle;
	text-align: center;
	-webkit-user-select: none;
	-moz-user-select: none;
	-ms-user-select: none;
	user-select: none;

	
	overflow: visible;
}

.ui-button,
.ui-button:link,
.ui-button:visited,
.ui-button:hover,
.ui-button:active {
	text-decoration: none;
}


.ui-button-icon-only {
	width: 2em;
	box-sizing: border-box;
	text-indent: -9999px;
	white-space: nowrap;
}


input.ui-button.ui-button-icon-only {
	text-indent: 0;
}


.ui-button-icon-only .ui-icon {
	position: absolute;
	top: 50%;
	left: 50%;
	margin-top: -8px;
	margin-left: -8px;
}

.ui-button.ui-icon-notext .ui-icon {
	padding: 0;
	width: 2.1em;
	height: 2.1em;
	text-indent: -9999px;
	white-space: nowrap;

}

input.ui-button.ui-icon-notext .ui-icon {
	width: auto;
	height: auto;
	text-indent: 0;
	white-space: normal;
	padding: .4em 1em;
}



input.ui-button::-moz-focus-inner,
button.ui-button::-moz-focus-inner {
	border: 0;
	padding: 0;
}
.ui-controlgroup {
	vertical-align: middle;
	display: inline-block;
}
.ui-controlgroup > .ui-controlgroup-item {
	float: left;
	margin-left: 0;
	margin-right: 0;
}
.ui-controlgroup > .ui-controlgroup-item:focus,
.ui-controlgroup > .ui-controlgroup-item.ui-visual-focus {
	z-index: 9999;
}
.ui-controlgroup-vertical > .ui-controlgroup-item {
	display: block;
	float: none;
	width: 100%;
	margin-top: 0;
	margin-bottom: 0;
	text-align: left;
}
.ui-controlgroup-vertical .ui-controlgroup-item {
	box-sizing: border-box;
}
.ui-controlgroup .ui-controlgroup-label {
	padding: .4em 1em;
}
.ui-controlgroup .ui-controlgroup-label span {
	font-size: 80%;
}
.ui-controlgroup-horizontal .ui-controlgroup-label + .ui-controlgroup-item {
	border-left: none;
}
.ui-controlgroup-vertical .ui-controlgroup-label + .ui-controlgroup-item {
	border-top: none;
}
.ui-controlgroup-horizontal .ui-controlgroup-label.ui-widget-content {
	border-right: none;
}
.ui-controlgroup-vertical .ui-controlgroup-label.ui-widget-content {
	border-bottom: none;
}


.ui-controlgroup-vertical .ui-spinner-input {

	
	width: 75%;
	width: calc( 100% - 2.4em );
}
.ui-controlgroup-vertical .ui-spinner .ui-spinner-up {
	border-top-style: solid;
}

.ui-checkboxradio-label .ui-icon-background {
	box-shadow: inset 1px 1px 1px #ccc;
	border-radius: .12em;
	border: none;
}
.ui-checkboxradio-radio-label .ui-icon-background {
	width: 16px;
	height: 16px;
	border-radius: 1em;
	overflow: visible;
	border: none;
}
.ui-checkboxradio-radio-label.ui-checkboxradio-checked .ui-icon,
.ui-checkboxradio-radio-label.ui-checkboxradio-checked:hover .ui-icon {
	background-image: none;
	width: 8px;
	height: 8px;
	border-width: 4px;
	border-style: solid;
}
.ui-checkboxradio-disabled {
	pointer-events: none;
}
.ui-datepicker {
	width: 17em;
	padding: .2em .2em 0;
	display: none;
}
.ui-datepicker .ui-datepicker-header {
	position: relative;
	padding: .2em 0;
}
.ui-datepicker .ui-datepicker-prev,
.ui-datepicker .ui-datepicker-next {
	position: absolute;
	top: 2px;
	width: 1.8em;
	height: 1.8em;
}
.ui-datepicker .ui-datepicker-prev-hover,
.ui-datepicker .ui-datepicker-next-hover {
	top: 1px;
}
.ui-datepicker .ui-datepicker-prev {
	left: 2px;
}
.ui-datepicker .ui-datepicker-next {
	right: 2px;
}
.ui-datepicker .ui-datepicker-prev-hover {
	left: 1px;
}
.ui-datepicker .ui-datepicker-next-hover {
	right: 1px;
}
.ui-datepicker .ui-datepicker-prev span,
.ui-datepicker .ui-datepicker-next span {
	display: block;
	position: absolute;
	left: 50%;
	margin-left: -8px;
	top: 50%;
	margin-top: -8px;
}
.ui-datepicker .ui-datepicker-title {
	margin: 0 2.3em;
	line-height: 1.8em;
	text-align: center;
}
.ui-datepicker .ui-datepicker-title select {
	font-size: 1em;
	margin: 1px 0;
}
.ui-datepicker select.ui-datepicker-month,
.ui-datepicker select.ui-datepicker-year {
	width: 45%;
}
.ui-datepicker table {
	width: 100%;
	font-size: .9em;
	border-collapse: collapse;
	margin: 0 0 .4em;
}
.ui-datepicker th {
	padding: .7em .3em;
	text-align: center;
	font-weight: bold;
	border: 0;
}
.ui-datepicker td {
	border: 0;
	padding: 1px;
}
.ui-datepicker td span,
.ui-datepicker td a {
	display: block;
	padding: .2em;
	text-align: right;
	text-decoration: none;
}
.ui-datepicker .ui-datepicker-buttonpane {
	background-image: none;
	margin: .7em 0 0 0;
	padding: 0 .2em;
	border-left: 0;
	border-right: 0;
	border-bottom: 0;
}
.ui-datepicker .ui-datepicker-buttonpane button {
	float: right;
	margin: .5em .2em .4em;
	cursor: pointer;
	padding: .2em .6em .3em .6em;
	width: auto;
	overflow: visible;
}
.ui-datepicker .ui-datepicker-buttonpane button.ui-datepicker-current {
	float: left;
}


.ui-datepicker.ui-datepicker-multi {
	width: auto;
}
.ui-datepicker-multi .ui-datepicker-group {
	float: left;
}
.ui-datepicker-multi .ui-datepicker-group table {
	width: 95%;
	margin: 0 auto .4em;
}
.ui-datepicker-multi-2 .ui-datepicker-group {
	width: 50%;
}
.ui-datepicker-multi-3 .ui-datepicker-group {
	width: 33.3%;
}
.ui-datepicker-multi-4 .ui-datepicker-group {
	width: 25%;
}
.ui-datepicker-multi .ui-datepicker-group-last .ui-datepicker-header,
.ui-datepicker-multi .ui-datepicker-group-middle .ui-datepicker-header {
	border-left-width: 0;
}
.ui-datepicker-multi .ui-datepicker-buttonpane {
	clear: left;
}
.ui-datepicker-row-break {
	clear: both;
	width: 100%;
	font-size: 0;
}


.ui-datepicker-rtl {
	direction: rtl;
}
.ui-datepicker-rtl .ui-datepicker-prev {
	right: 2px;
	left: auto;
}
.ui-datepicker-rtl .ui-datepicker-next {
	left: 2px;
	right: auto;
}
.ui-datepicker-rtl .ui-datepicker-prev:hover {
	right: 1px;
	left: auto;
}
.ui-datepicker-rtl .ui-datepicker-next:hover {
	left: 1px;
	right: auto;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane {
	clear: right;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane button {
	float: left;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane button.ui-datepicker-current,
.ui-datepicker-rtl .ui-datepicker-group {
	float: right;
}
.ui-datepicker-rtl .ui-datepicker-group-last .ui-datepicker-header,
.ui-datepicker-rtl .ui-datepicker-group-middle .ui-datepicker-header {
	border-right-width: 0;
	border-left-width: 1px;
}


.ui-datepicker .ui-icon {
	display: block;
	text-indent: -99999px;
	overflow: hidden;
	background-repeat: no-repeat;
	left: .5em;
	top: .3em;
}
.ui-dialog {
	position: absolute;
	top: 0;
	left: 0;
	padding: .2em;
	outline: 0;
}
.ui-dialog .ui-dialog-titlebar {
	padding: .4em 1em;
	position: relative;
}
.ui-dialog .ui-dialog-title {
	float: left;
	margin: .1em 0;
	white-space: nowrap;
	width: 90%;
	overflow: hidden;
	text-overflow: ellipsis;
}
.ui-dialog .ui-dialog-titlebar-close {
	position: absolute;
	right: .3em;
	top: 50%;
	width: 20px;
	margin: -10px 0 0 0;
	padding: 1px;
	height: 20px;
}
.ui-dialog .ui-dialog-content {
	position: relative;
	border: 0;
	padding: .5em 1em;
	background: none;
	overflow: auto;
}
.ui-dialog .ui-dialog-buttonpane {
	text-align: left;
	border-width: 1px 0 0 0;
	background-image: none;
	margin-top: .5em;
	padding: .3em 1em .5em .4em;
}
.ui-dialog .ui-dialog-buttonpane .ui-dialog-buttonset {
	float: right;
}
.ui-dialog .ui-dialog-buttonpane button {
	margin: .5em .4em .5em 0;
	cursor: pointer;
}
.ui-dialog .ui-resizable-n {
	height: 2px;
	top: 0;
}
.ui-dialog .ui-resizable-e {
	width: 2px;
	right: 0;
}
.ui-dialog .ui-resizable-s {
	height: 2px;
	bottom: 0;
}
.ui-dialog .ui-resizable-w {
	width: 2px;
	left: 0;
}
.ui-dialog .ui-resizable-se,
.ui-dialog .ui-resizable-sw,
.ui-dialog .ui-resizable-ne,
.ui-dialog .ui-resizable-nw {
	width: 7px;
	height: 7px;
}
.ui-dialog .ui-resizable-se {
	right: 0;
	bottom: 0;
}
.ui-dialog .ui-resizable-sw {
	left: 0;
	bottom: 0;
}
.ui-dialog .ui-resizable-ne {
	right: 0;
	top: 0;
}
.ui-dialog .ui-resizable-nw {
	left: 0;
	top: 0;
}
.ui-draggable .ui-dialog-titlebar {
	cursor: move;
}
.ui-draggable-handle {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-resizable {
	position: relative;
}
.ui-resizable-handle {
	position: absolute;
	font-size: 0.1px;
	display: block;
	-ms-touch-action: none;
	touch-action: none;
}
.ui-resizable-disabled .ui-resizable-handle,
.ui-resizable-autohide .ui-resizable-handle {
	display: none;
}
.ui-resizable-n {
	cursor: n-resize;
	height: 7px;
	width: 100%;
	top: -5px;
	left: 0;
}
.ui-resizable-s {
	cursor: s-resize;
	height: 7px;
	width: 100%;
	bottom: -5px;
	left: 0;
}
.ui-resizable-e {
	cursor: e-resize;
	width: 7px;
	right: -5px;
	top: 0;
	height: 100%;
}
.ui-resizable-w {
	cursor: w-resize;
	width: 7px;
	left: -5px;
	top: 0;
	height: 100%;
}
.ui-resizable-se {
	cursor: se-resize;
	width: 12px;
	height: 12px;
	right: 1px;
	bottom: 1px;
}
.ui-resizable-sw {
	cursor: sw-resize;
	width: 9px;
	height: 9px;
	left: -5px;
	bottom: -5px;
}
.ui-resizable-nw {
	cursor: nw-resize;
	width: 9px;
	height: 9px;
	left: -5px;
	top: -5px;
}
.ui-resizable-ne {
	cursor: ne-resize;
	width: 9px;
	height: 9px;
	right: -5px;
	top: -5px;
}
.ui-progressbar {
	height: 2em;
	text-align: left;
	overflow: hidden;
}
.ui-progressbar .ui-progressbar-value {
	margin: -1px;
	height: 100%;
}
.ui-progressbar .ui-progressbar-overlay {
	background: url("data:image/gif;base64,R0lGODlhKAAoAIABAAAAAP///yH/C05FVFNDQVBFMi4wAwEAAAAh+QQJAQABACwAAAAAKAAoAAACkYwNqXrdC52DS06a7MFZI+4FHBCKoDeWKXqymPqGqxvJrXZbMx7Ttc+w9XgU2FB3lOyQRWET2IFGiU9m1frDVpxZZc6bfHwv4c1YXP6k1Vdy292Fb6UkuvFtXpvWSzA+HycXJHUXiGYIiMg2R6W459gnWGfHNdjIqDWVqemH2ekpObkpOlppWUqZiqr6edqqWQAAIfkECQEAAQAsAAAAACgAKAAAApSMgZnGfaqcg1E2uuzDmmHUBR8Qil95hiPKqWn3aqtLsS18y7G1SzNeowWBENtQd+T1JktP05nzPTdJZlR6vUxNWWjV+vUWhWNkWFwxl9VpZRedYcflIOLafaa28XdsH/ynlcc1uPVDZxQIR0K25+cICCmoqCe5mGhZOfeYSUh5yJcJyrkZWWpaR8doJ2o4NYq62lAAACH5BAkBAAEALAAAAAAoACgAAAKVDI4Yy22ZnINRNqosw0Bv7i1gyHUkFj7oSaWlu3ovC8GxNso5fluz3qLVhBVeT/Lz7ZTHyxL5dDalQWPVOsQWtRnuwXaFTj9jVVh8pma9JjZ4zYSj5ZOyma7uuolffh+IR5aW97cHuBUXKGKXlKjn+DiHWMcYJah4N0lYCMlJOXipGRr5qdgoSTrqWSq6WFl2ypoaUAAAIfkECQEAAQAsAAAAACgAKAAAApaEb6HLgd/iO7FNWtcFWe+ufODGjRfoiJ2akShbueb0wtI50zm02pbvwfWEMWBQ1zKGlLIhskiEPm9R6vRXxV4ZzWT2yHOGpWMyorblKlNp8HmHEb/lCXjcW7bmtXP8Xt229OVWR1fod2eWqNfHuMjXCPkIGNileOiImVmCOEmoSfn3yXlJWmoHGhqp6ilYuWYpmTqKUgAAIfkECQEAAQAsAAAAACgAKAAAApiEH6kb58biQ3FNWtMFWW3eNVcojuFGfqnZqSebuS06w5V80/X02pKe8zFwP6EFWOT1lDFk8rGERh1TTNOocQ61Hm4Xm2VexUHpzjymViHrFbiELsefVrn6XKfnt2Q9G/+Xdie499XHd2g4h7ioOGhXGJboGAnXSBnoBwKYyfioubZJ2Hn0RuRZaflZOil56Zp6iioKSXpUAAAh+QQJAQABACwAAAAAKAAoAAACkoQRqRvnxuI7kU1a1UU5bd5tnSeOZXhmn5lWK3qNTWvRdQxP8qvaC+/yaYQzXO7BMvaUEmJRd3TsiMAgswmNYrSgZdYrTX6tSHGZO73ezuAw2uxuQ+BbeZfMxsexY35+/Qe4J1inV0g4x3WHuMhIl2jXOKT2Q+VU5fgoSUI52VfZyfkJGkha6jmY+aaYdirq+lQAACH5BAkBAAEALAAAAAAoACgAAAKWBIKpYe0L3YNKToqswUlvznigd4wiR4KhZrKt9Upqip61i9E3vMvxRdHlbEFiEXfk9YARYxOZZD6VQ2pUunBmtRXo1Lf8hMVVcNl8JafV38aM2/Fu5V16Bn63r6xt97j09+MXSFi4BniGFae3hzbH9+hYBzkpuUh5aZmHuanZOZgIuvbGiNeomCnaxxap2upaCZsq+1kAACH5BAkBAAEALAAAAAAoACgAAAKXjI8By5zf4kOxTVrXNVlv1X0d8IGZGKLnNpYtm8Lr9cqVeuOSvfOW79D9aDHizNhDJidFZhNydEahOaDH6nomtJjp1tutKoNWkvA6JqfRVLHU/QUfau9l2x7G54d1fl995xcIGAdXqMfBNadoYrhH+Mg2KBlpVpbluCiXmMnZ2Sh4GBqJ+ckIOqqJ6LmKSllZmsoq6wpQAAAh+QQJAQABACwAAAAAKAAoAAAClYx/oLvoxuJDkU1a1YUZbJ59nSd2ZXhWqbRa2/gF8Gu2DY3iqs7yrq+xBYEkYvFSM8aSSObE+ZgRl1BHFZNr7pRCavZ5BW2142hY3AN/zWtsmf12p9XxxFl2lpLn1rseztfXZjdIWIf2s5dItwjYKBgo9yg5pHgzJXTEeGlZuenpyPmpGQoKOWkYmSpaSnqKileI2FAAACH5BAkBAAEALAAAAAAoACgAAAKVjB+gu+jG4kORTVrVhRlsnn2dJ3ZleFaptFrb+CXmO9OozeL5VfP99HvAWhpiUdcwkpBH3825AwYdU8xTqlLGhtCosArKMpvfa1mMRae9VvWZfeB2XfPkeLmm18lUcBj+p5dnN8jXZ3YIGEhYuOUn45aoCDkp16hl5IjYJvjWKcnoGQpqyPlpOhr3aElaqrq56Bq7VAAAOw==");
	height: 100%;
	filter: alpha(opacity=25); 
	opacity: 0.25;
}
.ui-progressbar-indeterminate .ui-progressbar-value {
	background-image: none;
}
.ui-selectable {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-selectable-helper {
	position: absolute;
	z-index: 100;
	border: 1px dotted black;
}
.ui-selectmenu-menu {
	padding: 0;
	margin: 0;
	position: absolute;
	top: 0;
	left: 0;
	display: none;
}
.ui-selectmenu-menu .ui-menu {
	overflow: auto;
	overflow-x: hidden;
	padding-bottom: 1px;
}
.ui-selectmenu-menu .ui-menu .ui-selectmenu-optgroup {
	font-size: 1em;
	font-weight: bold;
	line-height: 1.5;
	padding: 2px 0.4em;
	margin: 0.5em 0 0 0;
	height: auto;
	border: 0;
}
.ui-selectmenu-open {
	display: block;
}
.ui-selectmenu-text {
	display: block;
	margin-right: 20px;
	overflow: hidden;
	text-overflow: ellipsis;
}
.ui-selectmenu-button.ui-button {
	text-align: left;
	white-space: nowrap;
	width: 14em;
}
.ui-selectmenu-icon.ui-icon {
	float: right;
	margin-top: 0;
}
.ui-slider {
	position: relative;
	text-align: left;
}
.ui-slider .ui-slider-handle {
	position: absolute;
	z-index: 2;
	width: 1.2em;
	height: 1.2em;
	cursor: default;
	-ms-touch-action: none;
	touch-action: none;
}
.ui-slider .ui-slider-range {
	position: absolute;
	z-index: 1;
	font-size: .7em;
	display: block;
	border: 0;
	background-position: 0 0;
}


.ui-slider.ui-state-disabled .ui-slider-handle,
.ui-slider.ui-state-disabled .ui-slider-range {
	filter: inherit;
}

.ui-slider-horizontal {
	height: .8em;
}
.ui-slider-horizontal .ui-slider-handle {
	top: -.3em;
	margin-left: -.6em;
}
.ui-slider-horizontal .ui-slider-range {
	top: 0;
	height: 100%;
}
.ui-slider-horizontal .ui-slider-range-min {
	left: 0;
}
.ui-slider-horizontal .ui-slider-range-max {
	right: 0;
}

.ui-slider-vertical {
	width: .8em;
	height: 100px;
}
.ui-slider-vertical .ui-slider-handle {
	left: -.3em;
	margin-left: 0;
	margin-bottom: -.6em;
}
.ui-slider-vertical .ui-slider-range {
	left: 0;
	width: 100%;
}
.ui-slider-vertical .ui-slider-range-min {
	bottom: 0;
}
.ui-slider-vertical .ui-slider-range-max {
	top: 0;
}
.ui-sortable-handle {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-spinner {
	position: relative;
	display: inline-block;
	overflow: hidden;
	padding: 0;
	vertical-align: middle;
}
.ui-spinner-input {
	border: none;
	background: none;
	color: inherit;
	padding: .222em 0;
	margin: .2em 0;
	vertical-align: middle;
	margin-left: .4em;
	margin-right: 2em;
}
.ui-spinner-button {
	width: 1.6em;
	height: 50%;
	font-size: .5em;
	padding: 0;
	margin: 0;
	text-align: center;
	position: absolute;
	cursor: default;
	display: block;
	overflow: hidden;
	right: 0;
}

.ui-spinner a.ui-spinner-button {
	border-top-style: none;
	border-bottom-style: none;
	border-right-style: none;
}
.ui-spinner-up {
	top: 0;
}
.ui-spinner-down {
	bottom: 0;
}
.ui-tabs {
	position: relative;
	padding: .2em;
}
.ui-tabs .ui-tabs-nav {
	margin: 0;
	padding: .2em .2em 0;
}
.ui-tabs .ui-tabs-nav li {
	list-style: none;
	float: left;
	position: relative;
	top: 0;
	margin: 1px .2em 0 0;
	border-bottom-width: 0;
	padding: 0;
	white-space: nowrap;
}
.ui-tabs .ui-tabs-nav .ui-tabs-anchor {
	float: left;
	padding: .5em 1em;
	text-decoration: none;
}
.ui-tabs .ui-tabs-nav li.ui-tabs-active {
	margin-bottom: -1px;
	padding-bottom: 1px;
}
.ui-tabs .ui-tabs-nav li.ui-tabs-active .ui-tabs-anchor,
.ui-tabs .ui-tabs-nav li.ui-state-disabled .ui-tabs-anchor,
.ui-tabs .ui-tabs-nav li.ui-tabs-loading .ui-tabs-anchor {
	cursor: text;
}
.ui-tabs-collapsible .ui-tabs-nav li.ui-tabs-active .ui-tabs-anchor {
	cursor: pointer;
}
.ui-tabs .ui-tabs-panel {
	display: block;
	border-width: 0;
	padding: 1em 1.4em;
	background: none;
}
.ui-tooltip {
	padding: 8px;
	position: absolute;
	z-index: 9999;
	max-width: 300px;
}
body .ui-tooltip {
	border-width: 2px;
}

.ui-widget {
	font-family: Arial,Helvetica,sans-serif;
	font-size: 1em;
}
.ui-widget .ui-widget {
	font-size: 1em;
}
.ui-widget input,
.ui-widget select,
.ui-widget textarea,
.ui-widget button {
	font-family: Arial,Helvetica,sans-serif;
	font-size: 1em;
}
.ui-widget.ui-widget-content {
	border: 1px solid #c5c5c5;
}
.ui-widget-content {
	border: 1px solid #dddddd;
	background: #ffffff;
	color: #333333;
}
.ui-widget-content a {
	color: #333333;
}
.ui-widget-header {
	border: 1px solid #dddddd;
	background: #e9e9e9;
	color: #333333;
	font-weight: bold;
}
.ui-widget-header a {
	color: #333333;
}


.ui-state-default,
.ui-widget-content .ui-state-default,
.ui-widget-header .ui-state-default,
.ui-button,


html .ui-button.ui-state-disabled:hover,
html .ui-button.ui-state-disabled:active {
	border: 1px solid #c5c5c5;
	background: #f6f6f6;
	font-weight: normal;
	color: #454545;
}
.ui-state-default a,
.ui-state-default a:link,
.ui-state-default a:visited,
a.ui-button,
a:link.ui-button,
a:visited.ui-button,
.ui-button {
	color: #454545;
	text-decoration: none;
}
.ui-state-hover,
.ui-widget-content .ui-state-hover,
.ui-widget-header .ui-state-hover,
.ui-state-focus,
.ui-widget-content .ui-state-focus,
.ui-widget-header .ui-state-focus,
.ui-button:hover,
.ui-button:focus {
	border: 1px solid #cccccc;
	background: #ededed;
	font-weight: normal;
	color: #2b2b2b;
}
.ui-state-hover a,
.ui-state-hover a:hover,
.ui-state-hover a:link,
.ui-state-hover a:visited,
.ui-state-focus a,
.ui-state-focus a:hover,
.ui-state-focus a:link,
.ui-state-focus a:visited,
a.ui-button:hover,
a.ui-button:focus {
	color: #2b2b2b;
	text-decoration: none;
}

.ui-visual-focus {
	box-shadow: 0 0 3px 1px rgb(94, 158, 214);
}
.ui-state-active,
.ui-widget-content .ui-state-active,
.ui-widget-header .ui-state-active,
a.ui-button:active,
.ui-button:active,
.ui-button.ui-state-active:hover {
	border: 1px solid #003eff;
	background: #007fff;
	font-weight: normal;
	color: #ffffff;
}
.ui-icon-background,
.ui-state-active .ui-icon-background {
	border: #003eff;
	background-color: #ffffff;
}
.ui-state-active a,
.ui-state-active a:link,
.ui-state-active a:visited {
	color: #ffffff;
	text-decoration: none;
}


.ui-state-highlight,
.ui-widget-content .ui-state-highlight,
.ui-widget-header .ui-state-highlight {
	border: 1px solid #dad55e;
	background: #fffa90;
	color: #777620;
}
.ui-state-checked {
	border: 1px solid #dad55e;
	background: #fffa90;
}
.ui-state-highlight a,
.ui-widget-content .ui-state-highlight a,
.ui-widget-header .ui-state-highlight a {
	color: #777620;
}
.ui-state-error,
.ui-widget-content .ui-state-error,
.ui-widget-header .ui-state-error {
	border: 1px solid #f1a899;
	background: #fddfdf;
	color: #5f3f3f;
}
.ui-state-error a,
.ui-widget-content .ui-state-error a,
.ui-widget-header .ui-state-error a {
	color: #5f3f3f;
}
.ui-state-error-text,
.ui-widget-content .ui-state-error-text,
.ui-widget-header .ui-state-error-text {
	color: #5f3f3f;
}
.ui-priority-primary,
.ui-widget-content .ui-priority-primary,
.ui-widget-header .ui-priority-primary {
	font-weight: bold;
}
.ui-priority-secondary,
.ui-widget-content .ui-priority-secondary,
.ui-widget-header .ui-priority-secondary {
	opacity: .7;
	filter:Alpha(Opacity=70); 
	font-weight: normal;
}
.ui-state-disabled,
.ui-widget-content .ui-state-disabled,
.ui-widget-header .ui-state-disabled {
	opacity: .35;
	filter:Alpha(Opacity=35); 
	background-image: none;
}
.ui-state-disabled .ui-icon {
	filter:Alpha(Opacity=35); 
}




.ui-icon {
	width: 16px;
	height: 16px;
}
.ui-icon,
.ui-widget-content .ui-icon {
	background-image: url("images/ui-icons_444444_256x240.png");
}
.ui-widget-header .ui-icon {
	background-image: url("images/ui-icons_444444_256x240.png");
}
.ui-state-hover .ui-icon,
.ui-state-focus .ui-icon,
.ui-button:hover .ui-icon,
.ui-button:focus .ui-icon {
	background-image: url("images/ui-icons_555555_256x240.png");
}
.ui-state-active .ui-icon,
.ui-button:active .ui-icon {
	background-image: url("images/ui-icons_ffffff_256x240.png");
}
.ui-state-highlight .ui-icon,
.ui-button .ui-state-highlight.ui-icon {
	background-image: url("images/ui-icons_777620_256x240.png");
}
.ui-state-error .ui-icon,
.ui-state-error-text .ui-icon {
	background-image: url("images/ui-icons_cc0000_256x240.png");
}
.ui-button .ui-icon {
	background-image: url("images/ui-icons_777777_256x240.png");
}


.ui-icon-blank { background-position: 16px 16px; }
.ui-icon-caret-1-n { background-position: 0 0; }
.ui-icon-caret-1-ne { background-position: -16px 0; }
.ui-icon-caret-1-e { background-position: -32px 0; }
.ui-icon-caret-1-se { background-position: -48px 0; }
.ui-icon-caret-1-s { background-position: -65px 0; }
.ui-icon-caret-1-sw { background-position: -80px 0; }
.ui-icon-caret-1-w { background-position: -96px 0; }
.ui-icon-caret-1-nw { background-position: -112px 0; }
.ui-icon-caret-2-n-s { background-position: -128px 0; }
.ui-icon-caret-2-e-w { background-position: -144px 0; }
.ui-icon-triangle-1-n { background-position: 0 -16px; }
.ui-icon-triangle-1-ne { background-position: -16px -16px; }
.ui-icon-triangle-1-e { background-position: -32px -16px; }
.ui-icon-triangle-1-se { background-position: -48px -16px; }
.ui-icon-triangle-1-s { background-position: -65px -16px; }
.ui-icon-triangle-1-sw { background-position: -80px -16px; }
.ui-icon-triangle-1-w { background-position: -96px -16px; }
.ui-icon-triangle-1-nw { background-position: -112px -16px; }
.ui-icon-triangle-2-n-s { background-position: -128px -16px; }
.ui-icon-triangle-2-e-w { background-position: -144px -16px; }
.ui-icon-arrow-1-n { background-position: 0 -32px; }
.ui-icon-arrow-1-ne { background-position: -16px -32px; }
.ui-icon-arrow-1-e { background-position: -32px -32px; }
.ui-icon-arrow-1-se { background-position: -48px -32px; }
.ui-icon-arrow-1-s { background-position: -65px -32px; }
.ui-icon-arrow-1-sw { background-position: -80px -32px; }
.ui-icon-arrow-1-w { background-position: -96px -32px; }
.ui-icon-arrow-1-nw { background-position: -112px -32px; }
.ui-icon-arrow-2-n-s { background-position: -128px -32px; }
.ui-icon-arrow-2-ne-sw { background-position: -144px -32px; }
.ui-icon-arrow-2-e-w { background-position: -160px -32px; }
.ui-icon-arrow-2-se-nw { background-position: -176px -32px; }
.ui-icon-arrowstop-1-n { background-position: -192px -32px; }
.ui-icon-arrowstop-1-e { background-position: -208px -32px; }
.ui-icon-arrowstop-1-s { background-position: -224px -32px; }
.ui-icon-arrowstop-1-w { background-position: -240px -32px; }
.ui-icon-arrowthick-1-n { background-position: 1px -48px; }
.ui-icon-arrowthick-1-ne { background-position: -16px -48px; }
.ui-icon-arrowthick-1-e { background-position: -32px -48px; }
.ui-icon-arrowthick-1-se { background-position: -48px -48px; }
.ui-icon-arrowthick-1-s { background-position: -64px -48px; }
.ui-icon-arrowthick-1-sw { background-position: -80px -48px; }
.ui-icon-arrowthick-1-w { background-position: -96px -48px; }
.ui-icon-arrowthick-1-nw { background-position: -112px -48px; }
.ui-icon-arrowthick-2-n-s { background-position: -128px -48px; }
.ui-icon-arrowthick-2-ne-sw { background-position: -144px -48px; }
.ui-icon-arrowthick-2-e-w { background-position: -160px -48px; }
.ui-icon-arrowthick-2-se-nw { background-position: -176px -48px; }
.ui-icon-arrowthickstop-1-n { background-position: -192px -48px; }
.ui-icon-arrowthickstop-1-e { background-position: -208px -48px; }
.ui-icon-arrowthickstop-1-s { background-position: -224px -48px; }
.ui-icon-arrowthickstop-1-w { background-position: -240px -48px; }
.ui-icon-arrowreturnthick-1-w { background-position: 0 -64px; }
.ui-icon-arrowreturnthick-1-n { background-position: -16px -64px; }
.ui-icon-arrowreturnthick-1-e { background-position: -32px -64px; }
.ui-icon-arrowreturnthick-1-s { background-position: -48px -64px; }
.ui-icon-arrowreturn-1-w { background-position: -64px -64px; }
.ui-icon-arrowreturn-1-n { background-position: -80px -64px; }
.ui-icon-arrowreturn-1-e { background-position: -96px -64px; }
.ui-icon-arrowreturn-1-s { background-position: -112px -64px; }
.ui-icon-arrowrefresh-1-w { background-position: -128px -64px; }
.ui-icon-arrowrefresh-1-n { background-position: -144px -64px; }
.ui-icon-arrowrefresh-1-e { background-position: -160px -64px; }
.ui-icon-arrowrefresh-1-s { background-position: -176px -64px; }
.ui-icon-arrow-4 { background-position: 0 -80px; }
.ui-icon-arrow-4-diag { background-position: -16px -80px; }
.ui-icon-extlink { background-position: -32px -80px; }
.ui-icon-newwin { background-position: -48px -80px; }
.ui-icon-refresh { background-position: -64px -80px; }
.ui-icon-shuffle { background-position: -80px -80px; }
.ui-icon-transfer-e-w { background-position: -96px -80px; }
.ui-icon-transferthick-e-w { background-position: -112px -80px; }
.ui-icon-folder-collapsed { background-position: 0 -96px; }
.ui-icon-folder-open { background-position: -16px -96px; }
.ui-icon-document { background-position: -32px -96px; }
.ui-icon-document-b { background-position: -48px -96px; }
.ui-icon-note { background-position: -64px -96px; }
.ui-icon-mail-closed { background-position: -80px -96px; }
.ui-icon-mail-open { background-position: -96px -96px; }
.ui-icon-suitcase { background-position: -112px -96px; }
.ui-icon-comment { background-position: -128px -96px; }
.ui-icon-person { background-position: -144px -96px; }
.ui-icon-print { background-position: -160px -96px; }
.ui-icon-trash { background-position: -176px -96px; }
.ui-icon-locked { background-position: -192px -96px; }
.ui-icon-unlocked { background-position: -208px -96px; }
.ui-icon-bookmark { background-position: -224px -96px; }
.ui-icon-tag { background-position: -240px -96px; }
.ui-icon-home { background-position: 0 -112px; }
.ui-icon-flag { background-position: -16px -112px; }
.ui-icon-calendar { background-position: -32px -112px; }
.ui-icon-cart { background-position: -48px -112px; }
.ui-icon-pencil { background-position: -64px -112px; }
.ui-icon-clock { background-position: -80px -112px; }
.ui-icon-disk { background-position: -96px -112px; }
.ui-icon-calculator { background-position: -112px -112px; }
.ui-icon-zoomin { background-position: -128px -112px; }
.ui-icon-zoomout { background-position: -144px -112px; }
.ui-icon-search { background-position: -160px -112px; }
.ui-icon-wrench { background-position: -176px -112px; }
.ui-icon-gear { background-position: -192px -112px; }
.ui-icon-heart { background-position: -208px -112px; }
.ui-icon-star { background-position: -224px -112px; }
.ui-icon-link { background-position: -240px -112px; }
.ui-icon-cancel { background-position: 0 -128px; }
.ui-icon-plus { background-position: -16px -128px; }
.ui-icon-plusthick { background-position: -32px -128px; }
.ui-icon-minus { background-position: -48px -128px; }
.ui-icon-minusthick { background-position: -64px -128px; }
.ui-icon-close { background-position: -80px -128px; }
.ui-icon-closethick { background-position: -96px -128px; }
.ui-icon-key { background-position: -112px -128px; }
.ui-icon-lightbulb { background-position: -128px -128px; }
.ui-icon-scissors { background-position: -144px -128px; }
.ui-icon-clipboard { background-position: -160px -128px; }
.ui-icon-copy { background-position: -176px -128px; }
.ui-icon-contact { background-position: -192px -128px; }
.ui-icon-image { background-position: -208px -128px; }
.ui-icon-video { background-position: -224px -128px; }
.ui-icon-script { background-position: -240px -128px; }
.ui-icon-alert { background-position: 0 -144px; }
.ui-icon-info { background-position: -16px -144px; }
.ui-icon-notice { background-position: -32px -144px; }
.ui-icon-help { background-position: -48px -144px; }
.ui-icon-check { background-position: -64px -144px; }
.ui-icon-bullet { background-position: -80px -144px; }
.ui-icon-radio-on { background-position: -96px -144px; }
.ui-icon-radio-off { background-position: -112px -144px; }
.ui-icon-pin-w { background-position: -128px -144px; }
.ui-icon-pin-s { background-position: -144px -144px; }
.ui-icon-play { background-position: 0 -160px; }
.ui-icon-pause { background-position: -16px -160px; }
.ui-icon-seek-next { background-position: -32px -160px; }
.ui-icon-seek-prev { background-position: -48px -160px; }
.ui-icon-seek-end { background-position: -64px -160px; }
.ui-icon-seek-start { background-position: -80px -160px; }

.ui-icon-seek-first { background-position: -80px -160px; }
.ui-icon-stop { background-position: -96px -160px; }
.ui-icon-eject { background-position: -112px -160px; }
.ui-icon-volume-off { background-position: -128px -160px; }
.ui-icon-volume-on { background-position: -144px -160px; }
.ui-icon-power { background-position: 0 -176px; }
.ui-icon-signal-diag { background-position: -16px -176px; }
.ui-icon-signal { background-position: -32px -176px; }
.ui-icon-battery-0 { background-position: -48px -176px; }
.ui-icon-battery-1 { background-position: -64px -176px; }
.ui-icon-battery-2 { background-position: -80px -176px; }
.ui-icon-battery-3 { background-position: -96px -176px; }
.ui-icon-circle-plus { background-position: 0 -192px; }
.ui-icon-circle-minus { background-position: -16px -192px; }
.ui-icon-circle-close { background-position: -32px -192px; }
.ui-icon-circle-triangle-e { background-position: -48px -192px; }
.ui-icon-circle-triangle-s { background-position: -64px -192px; }
.ui-icon-circle-triangle-w { background-position: -80px -192px; }
.ui-icon-circle-triangle-n { background-position: -96px -192px; }
.ui-icon-circle-arrow-e { background-position: -112px -192px; }
.ui-icon-circle-arrow-s { background-position: -128px -192px; }
.ui-icon-circle-arrow-w { background-position: -144px -192px; }
.ui-icon-circle-arrow-n { background-position: -160px -192px; }
.ui-icon-circle-zoomin { background-position: -176px -192px; }
.ui-icon-circle-zoomout { background-position: -192px -192px; }
.ui-icon-circle-check { background-position: -208px -192px; }
.ui-icon-circlesmall-plus { background-position: 0 -208px; }
.ui-icon-circlesmall-minus { background-position: -16px -208px; }
.ui-icon-circlesmall-close { background-position: -32px -208px; }
.ui-icon-squaresmall-plus { background-position: -48px -208px; }
.ui-icon-squaresmall-minus { background-position: -64px -208px; }
.ui-icon-squaresmall-close { background-position: -80px -208px; }
.ui-icon-grip-dotted-vertical { background-position: 0 -224px; }
.ui-icon-grip-dotted-horizontal { background-position: -16px -224px; }
.ui-icon-grip-solid-vertical { background-position: -32px -224px; }
.ui-icon-grip-solid-horizontal { background-position: -48px -224px; }
.ui-icon-gripsmall-diagonal-se { background-position: -64px -224px; }
.ui-icon-grip-diagonal-se { background-position: -80px -224px; }





.ui-corner-all,
.ui-corner-top,
.ui-corner-left,
.ui-corner-tl {
	border-top-left-radius: 3px;
}
.ui-corner-all,
.ui-corner-top,
.ui-corner-right,
.ui-corner-tr {
	border-top-right-radius: 3px;
}
.ui-corner-all,
.ui-corner-bottom,
.ui-corner-left,
.ui-corner-bl {
	border-bottom-left-radius: 3px;
}
.ui-corner-all,
.ui-corner-bottom,
.ui-corner-right,
.ui-corner-br {
	border-bottom-right-radius: 3px;
}


.ui-widget-overlay {
	background: #aaaaaa;
	opacity: .3;
	filter: Alpha(Opacity=30); 
}
.ui-widget-shadow {
	-webkit-box-shadow: 0px 0px 5px #666666;
	box-shadow: 0px 0px 5px #666666;
} 
</style>
<script src='js/jquery-1.12.4.js'></script>
<script src='js/jquery-ui.js'></script>

<script>
$( function() {
	$( '#tabs-collect-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-collect-consensus-heatmap'>
<ul>
<li><a href='#tab-collect-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-collect-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-collect-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-collect-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-collect-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-collect-consensus-heatmap-1'>
<pre><code class="r">collect_plots(res_list, k = 2, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-1-1.png" alt="plot of chunk tab-collect-consensus-heatmap-1"/></p>

</div>
<div id='tab-collect-consensus-heatmap-2'>
<pre><code class="r">collect_plots(res_list, k = 3, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-2-1.png" alt="plot of chunk tab-collect-consensus-heatmap-2"/></p>

</div>
<div id='tab-collect-consensus-heatmap-3'>
<pre><code class="r">collect_plots(res_list, k = 4, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-3-1.png" alt="plot of chunk tab-collect-consensus-heatmap-3"/></p>

</div>
<div id='tab-collect-consensus-heatmap-4'>
<pre><code class="r">collect_plots(res_list, k = 5, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-4-1.png" alt="plot of chunk tab-collect-consensus-heatmap-4"/></p>

</div>
<div id='tab-collect-consensus-heatmap-5'>
<pre><code class="r">collect_plots(res_list, k = 6, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-5-1.png" alt="plot of chunk tab-collect-consensus-heatmap-5"/></p>

</div>
</div>



### Membership heatmap

Membership heatmaps for all methods. ([What is a membership heatmap?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_12))


<script>
$( function() {
	$( '#tabs-collect-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-collect-membership-heatmap'>
<ul>
<li><a href='#tab-collect-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-collect-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-collect-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-collect-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-collect-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-collect-membership-heatmap-1'>
<pre><code class="r">collect_plots(res_list, k = 2, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-1-1.png" alt="plot of chunk tab-collect-membership-heatmap-1"/></p>

</div>
<div id='tab-collect-membership-heatmap-2'>
<pre><code class="r">collect_plots(res_list, k = 3, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-2-1.png" alt="plot of chunk tab-collect-membership-heatmap-2"/></p>

</div>
<div id='tab-collect-membership-heatmap-3'>
<pre><code class="r">collect_plots(res_list, k = 4, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-3-1.png" alt="plot of chunk tab-collect-membership-heatmap-3"/></p>

</div>
<div id='tab-collect-membership-heatmap-4'>
<pre><code class="r">collect_plots(res_list, k = 5, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-4-1.png" alt="plot of chunk tab-collect-membership-heatmap-4"/></p>

</div>
<div id='tab-collect-membership-heatmap-5'>
<pre><code class="r">collect_plots(res_list, k = 6, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-5-1.png" alt="plot of chunk tab-collect-membership-heatmap-5"/></p>

</div>
</div>



### Signature heatmap

Signature heatmaps for all methods. ([What is a signature heatmap?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_22))


Note in following heatmaps, rows are scaled.



<script>
$( function() {
	$( '#tabs-collect-get-signatures' ).tabs();
} );
</script>
<div id='tabs-collect-get-signatures'>
<ul>
<li><a href='#tab-collect-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-collect-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-collect-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-collect-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-collect-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-collect-get-signatures-1'>
<pre><code class="r">collect_plots(res_list, k = 2, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-1-1.png" alt="plot of chunk tab-collect-get-signatures-1"/></p>

</div>
<div id='tab-collect-get-signatures-2'>
<pre><code class="r">collect_plots(res_list, k = 3, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-2-1.png" alt="plot of chunk tab-collect-get-signatures-2"/></p>

</div>
<div id='tab-collect-get-signatures-3'>
<pre><code class="r">collect_plots(res_list, k = 4, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-3-1.png" alt="plot of chunk tab-collect-get-signatures-3"/></p>

</div>
<div id='tab-collect-get-signatures-4'>
<pre><code class="r">collect_plots(res_list, k = 5, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-4-1.png" alt="plot of chunk tab-collect-get-signatures-4"/></p>

</div>
<div id='tab-collect-get-signatures-5'>
<pre><code class="r">collect_plots(res_list, k = 6, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-5-1.png" alt="plot of chunk tab-collect-get-signatures-5"/></p>

</div>
</div>



### Statistics table

The statistics used for measuring the stability of consensus partitioning.
([How are they
defined?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13))


<script>
$( function() {
	$( '#tabs-get-stats-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-get-stats-from-consensus-partition-list'>
<ul>
<li><a href='#tab-get-stats-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-get-stats-from-consensus-partition-list-1'>
<pre><code class="r">get_stats(res_list, k = 2)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      2 0.960           0.951       0.978          0.408 0.609   0.609
#&gt; CV:NMF      2 0.887           0.937       0.972          0.421 0.591   0.591
#&gt; MAD:NMF     2 0.885           0.920       0.967          0.434 0.575   0.575
#&gt; ATC:NMF     2 1.000           1.000       1.000          0.373 0.628   0.628
#&gt; SD:skmeans  2 0.925           0.947       0.977          0.451 0.547   0.547
#&gt; CV:skmeans  2 0.742           0.923       0.963          0.471 0.525   0.525
#&gt; MAD:skmeans 2 0.811           0.888       0.954          0.470 0.535   0.535
#&gt; ATC:skmeans 2 1.000           0.974       0.991          0.433 0.560   0.560
#&gt; SD:mclust   2 1.000           0.978       0.990          0.470 0.535   0.535
#&gt; CV:mclust   2 0.826           0.930       0.967          0.438 0.547   0.547
#&gt; MAD:mclust  2 0.827           0.971       0.983          0.442 0.547   0.547
#&gt; ATC:mclust  2 1.000           0.977       0.989          0.479 0.516   0.516
#&gt; SD:kmeans   2 0.543           0.953       0.940          0.345 0.628   0.628
#&gt; CV:kmeans   2 0.547           0.958       0.937          0.337 0.628   0.628
#&gt; MAD:kmeans  2 0.547           0.964       0.946          0.341 0.628   0.628
#&gt; ATC:kmeans  2 1.000           0.997       0.994          0.371 0.628   0.628
#&gt; SD:pam      2 0.581           0.936       0.910          0.316 0.669   0.669
#&gt; CV:pam      2 1.000           0.962       0.984          0.323 0.669   0.669
#&gt; MAD:pam     2 1.000           0.963       0.987          0.338 0.669   0.669
#&gt; ATC:pam     2 1.000           0.999       0.999          0.372 0.628   0.628
#&gt; SD:hclust   2 1.000           0.980       0.989          0.375 0.628   0.628
#&gt; CV:hclust   2 0.866           0.921       0.958          0.368 0.628   0.628
#&gt; MAD:hclust  2 0.926           0.955       0.978          0.378 0.609   0.609
#&gt; ATC:hclust  2 1.000           0.978       0.987          0.366 0.628   0.628
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-2'>
<pre><code class="r">get_stats(res_list, k = 3)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      3 0.944           0.924       0.967          0.398 0.821   0.706
#&gt; CV:NMF      3 0.896           0.891       0.949          0.394 0.781   0.642
#&gt; MAD:NMF     3 0.854           0.895       0.949          0.379 0.758   0.606
#&gt; ATC:NMF     3 0.968           0.961       0.976          0.260 0.893   0.831
#&gt; SD:skmeans  3 0.851           0.884       0.949          0.488 0.704   0.494
#&gt; CV:skmeans  3 0.672           0.814       0.905          0.424 0.709   0.490
#&gt; MAD:skmeans 3 0.887           0.892       0.952          0.437 0.720   0.509
#&gt; ATC:skmeans 3 0.658           0.808       0.836          0.381 0.781   0.615
#&gt; SD:mclust   3 0.978           0.929       0.963          0.106 0.937   0.883
#&gt; CV:mclust   3 0.957           0.947       0.971          0.163 0.955   0.917
#&gt; MAD:mclust  3 1.000           0.973       0.988          0.128 0.955   0.917
#&gt; ATC:mclust  3 0.792           0.911       0.922          0.223 0.893   0.796
#&gt; SD:kmeans   3 0.859           0.961       0.968          0.448 0.874   0.800
#&gt; CV:kmeans   3 0.869           0.937       0.946          0.487 0.874   0.800
#&gt; MAD:kmeans  3 0.709           0.885       0.920          0.559 0.874   0.800
#&gt; ATC:kmeans  3 0.633           0.788       0.857          0.413 0.874   0.800
#&gt; SD:pam      3 0.806           0.916       0.943          0.532 0.867   0.802
#&gt; CV:pam      3 0.566           0.839       0.885          0.560 0.867   0.802
#&gt; MAD:pam     3 0.671           0.826       0.874          0.549 0.834   0.754
#&gt; ATC:pam     3 0.573           0.634       0.847          0.378 0.945   0.913
#&gt; SD:hclust   3 0.854           0.961       0.960          0.276 0.874   0.800
#&gt; CV:hclust   3 0.870           0.914       0.968          0.352 0.874   0.800
#&gt; MAD:hclust  3 0.964           0.944       0.980          0.333 0.878   0.799
#&gt; ATC:hclust  3 0.908           0.920       0.951          0.116 0.971   0.953
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-3'>
<pre><code class="r">get_stats(res_list, k = 4)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      4 0.726           0.842       0.902         0.1439 0.969   0.927
#&gt; CV:NMF      4 0.642           0.804       0.885         0.1323 0.944   0.866
#&gt; MAD:NMF     4 0.620           0.732       0.846         0.1499 0.941   0.860
#&gt; ATC:NMF     4 0.710           0.839       0.903         0.2901 0.823   0.681
#&gt; SD:skmeans  4 0.877           0.903       0.949         0.1116 0.879   0.655
#&gt; CV:skmeans  4 0.624           0.733       0.841         0.1212 0.878   0.652
#&gt; MAD:skmeans 4 0.759           0.820       0.900         0.1102 0.882   0.660
#&gt; ATC:skmeans 4 0.624           0.762       0.863         0.1387 0.924   0.791
#&gt; SD:mclust   4 0.862           0.925       0.935         0.1002 0.983   0.963
#&gt; CV:mclust   4 0.643           0.805       0.844         0.1934 0.979   0.958
#&gt; MAD:mclust  4 0.685           0.671       0.808         0.2373 0.899   0.801
#&gt; ATC:mclust  4 0.806           0.925       0.937         0.0454 0.976   0.942
#&gt; SD:kmeans   4 0.746           0.878       0.719         0.2691 0.774   0.550
#&gt; CV:kmeans   4 0.723           0.702       0.699         0.2784 0.767   0.538
#&gt; MAD:kmeans  4 0.723           0.908       0.830         0.2410 0.774   0.549
#&gt; ATC:kmeans  4 0.615           0.747       0.814         0.1741 0.830   0.662
#&gt; SD:pam      4 0.867           0.907       0.955         0.4619 0.751   0.535
#&gt; CV:pam      4 0.590           0.741       0.863         0.3689 0.727   0.504
#&gt; MAD:pam     4 0.820           0.831       0.933         0.3548 0.741   0.513
#&gt; ATC:pam     4 0.582           0.635       0.798         0.1291 0.876   0.793
#&gt; SD:hclust   4 0.944           0.913       0.971         0.1018 0.979   0.958
#&gt; CV:hclust   4 0.991           0.927       0.973         0.0937 0.948   0.896
#&gt; MAD:hclust  4 0.972           0.916       0.969         0.0681 0.969   0.935
#&gt; ATC:hclust  4 1.000           0.966       0.986         0.0308 0.992   0.986
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-4'>
<pre><code class="r">get_stats(res_list, k = 5)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      5 0.696           0.765       0.832         0.1242 0.819   0.555
#&gt; CV:NMF      5 0.591           0.560       0.772         0.1073 0.888   0.713
#&gt; MAD:NMF     5 0.705           0.693       0.826         0.1055 0.814   0.525
#&gt; ATC:NMF     5 0.668           0.736       0.865         0.0548 0.994   0.984
#&gt; SD:skmeans  5 0.723           0.668       0.823         0.0624 0.977   0.912
#&gt; CV:skmeans  5 0.605           0.547       0.741         0.0616 0.966   0.870
#&gt; MAD:skmeans 5 0.664           0.602       0.788         0.0638 0.991   0.964
#&gt; ATC:skmeans 5 0.613           0.630       0.821         0.0770 0.896   0.700
#&gt; SD:mclust   5 0.642           0.692       0.823         0.2399 0.777   0.521
#&gt; CV:mclust   5 0.619           0.620       0.841         0.1768 0.783   0.550
#&gt; MAD:mclust  5 0.796           0.746       0.871         0.1138 0.800   0.542
#&gt; ATC:mclust  5 0.725           0.859       0.873         0.1410 0.878   0.694
#&gt; SD:kmeans   5 0.709           0.947       0.902         0.1606 0.959   0.855
#&gt; CV:kmeans   5 0.671           0.894       0.877         0.1461 0.882   0.628
#&gt; MAD:kmeans  5 0.919           0.950       0.929         0.1328 0.960   0.857
#&gt; ATC:kmeans  5 0.638           0.731       0.814         0.1362 0.932   0.801
#&gt; SD:pam      5 0.845           0.894       0.957         0.0564 0.965   0.879
#&gt; CV:pam      5 0.623           0.726       0.857         0.0656 0.917   0.736
#&gt; MAD:pam     5 0.944           0.891       0.957         0.0429 0.941   0.806
#&gt; ATC:pam     5 0.539           0.605       0.774         0.1822 0.742   0.497
#&gt; SD:hclust   5 0.727           0.914       0.916         0.0829 0.992   0.983
#&gt; CV:hclust   5 0.803           0.844       0.907         0.0797 0.994   0.988
#&gt; MAD:hclust  5 0.696           0.809       0.889         0.1076 0.991   0.980
#&gt; ATC:hclust  5 0.656           0.835       0.890         0.3216 0.874   0.787
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-5'>
<pre><code class="r">get_stats(res_list, k = 6)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      6 0.875           0.795       0.901         0.0767 0.938   0.755
#&gt; CV:NMF      6 0.747           0.762       0.860         0.0902 0.826   0.482
#&gt; MAD:NMF     6 0.849           0.747       0.874         0.0553 0.924   0.708
#&gt; ATC:NMF     6 0.568           0.662       0.790         0.0862 0.956   0.891
#&gt; SD:skmeans  6 0.705           0.520       0.724         0.0457 0.977   0.907
#&gt; CV:skmeans  6 0.640           0.427       0.677         0.0400 0.967   0.856
#&gt; MAD:skmeans 6 0.664           0.528       0.715         0.0409 0.948   0.793
#&gt; ATC:skmeans 6 0.631           0.586       0.764         0.0550 0.977   0.922
#&gt; SD:mclust   6 0.615           0.553       0.749         0.0953 0.818   0.437
#&gt; CV:mclust   6 0.637           0.446       0.731         0.0952 0.892   0.647
#&gt; MAD:mclust  6 0.696           0.667       0.778         0.1241 0.931   0.765
#&gt; ATC:mclust  6 0.675           0.838       0.872         0.0759 0.992   0.970
#&gt; SD:kmeans   6 0.788           0.738       0.768         0.0781 0.951   0.797
#&gt; CV:kmeans   6 0.760           0.736       0.849         0.0687 0.988   0.951
#&gt; MAD:kmeans  6 0.805           0.862       0.872         0.0552 1.000   1.000
#&gt; ATC:kmeans  6 0.705           0.730       0.837         0.0783 0.886   0.644
#&gt; SD:pam      6 0.811           0.822       0.912         0.0294 0.974   0.900
#&gt; CV:pam      6 0.669           0.727       0.881         0.0296 0.983   0.934
#&gt; MAD:pam     6 0.853           0.841       0.911         0.0324 0.983   0.936
#&gt; ATC:pam     6 0.721           0.732       0.885         0.1035 0.869   0.587
#&gt; SD:hclust   6 0.708           0.785       0.811         0.1162 1.000   1.000
#&gt; CV:hclust   6 0.589           0.733       0.815         0.0989 0.971   0.935
#&gt; MAD:hclust  6 0.676           0.699       0.753         0.1601 0.776   0.521
#&gt; ATC:hclust  6 0.695           0.844       0.886         0.0682 0.971   0.937
</code></pre>

</div>
</div>

Following heatmap plots the partition for each combination of methods and the
lightness correspond to the silhouette scores for samples in each method. On
top the consensus subgroup is inferred from all methods by taking the mean
silhouette scores as weight.


<script>
$( function() {
	$( '#tabs-collect-stats-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-collect-stats-from-consensus-partition-list'>
<ul>
<li><a href='#tab-collect-stats-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-collect-stats-from-consensus-partition-list-1'>
<pre><code class="r">collect_stats(res_list, k = 2)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-1-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-1"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-2'>
<pre><code class="r">collect_stats(res_list, k = 3)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-2-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-2"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-3'>
<pre><code class="r">collect_stats(res_list, k = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-3-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-3"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-4'>
<pre><code class="r">collect_stats(res_list, k = 5)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-4-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-4"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-5'>
<pre><code class="r">collect_stats(res_list, k = 6)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-5-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-5"/></p>

</div>
</div>

### Partition from all methods



Collect partitions from all methods:


<script>
$( function() {
	$( '#tabs-collect-classes-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-collect-classes-from-consensus-partition-list'>
<ul>
<li><a href='#tab-collect-classes-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-collect-classes-from-consensus-partition-list-1'>
<pre><code class="r">collect_classes(res_list, k = 2)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-1-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-1"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-2'>
<pre><code class="r">collect_classes(res_list, k = 3)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-2-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-2"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-3'>
<pre><code class="r">collect_classes(res_list, k = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-3-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-3"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-4'>
<pre><code class="r">collect_classes(res_list, k = 5)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-4-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-4"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-5'>
<pre><code class="r">collect_classes(res_list, k = 6)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-5-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-5"/></p>

</div>
</div>



### Top rows overlap


Overlap of top rows from different top-row methods:


<script>
$( function() {
	$( '#tabs-top-rows-overlap-by-euler' ).tabs();
} );
</script>
<div id='tabs-top-rows-overlap-by-euler'>
<ul>
<li><a href='#tab-top-rows-overlap-by-euler-1'>top_n = 1000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-2'>top_n = 2000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-3'>top_n = 3000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-4'>top_n = 4000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-5'>top_n = 5000</a></li>
</ul>
<div id='tab-top-rows-overlap-by-euler-1'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 1000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-1-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-1"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-2'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 2000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-2-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-2"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-3'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 3000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-3-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-3"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-4'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 4000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-4-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-4"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-5'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 5000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-5-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-5"/></p>

</div>
</div>

Also visualize the correspondance of rankings between different top-row methods:


<script>
$( function() {
	$( '#tabs-top-rows-overlap-by-correspondance' ).tabs();
} );
</script>
<div id='tabs-top-rows-overlap-by-correspondance'>
<ul>
<li><a href='#tab-top-rows-overlap-by-correspondance-1'>top_n = 1000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-2'>top_n = 2000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-3'>top_n = 3000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-4'>top_n = 4000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-5'>top_n = 5000</a></li>
</ul>
<div id='tab-top-rows-overlap-by-correspondance-1'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 1000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-1-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-1"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-2'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 2000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-2-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-2"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-3'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 3000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-3-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-3"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-4'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 4000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-4-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-4"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-5'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 5000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-5-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-5"/></p>

</div>
</div>


Heatmaps of the top rows:



<script>
$( function() {
	$( '#tabs-top-rows-heatmap' ).tabs();
} );
</script>
<div id='tabs-top-rows-heatmap'>
<ul>
<li><a href='#tab-top-rows-heatmap-1'>top_n = 1000</a></li>
<li><a href='#tab-top-rows-heatmap-2'>top_n = 2000</a></li>
<li><a href='#tab-top-rows-heatmap-3'>top_n = 3000</a></li>
<li><a href='#tab-top-rows-heatmap-4'>top_n = 4000</a></li>
<li><a href='#tab-top-rows-heatmap-5'>top_n = 5000</a></li>
</ul>
<div id='tab-top-rows-heatmap-1'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 1000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-1-1.png" alt="plot of chunk tab-top-rows-heatmap-1"/></p>

</div>
<div id='tab-top-rows-heatmap-2'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 2000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-2-1.png" alt="plot of chunk tab-top-rows-heatmap-2"/></p>

</div>
<div id='tab-top-rows-heatmap-3'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 3000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-3-1.png" alt="plot of chunk tab-top-rows-heatmap-3"/></p>

</div>
<div id='tab-top-rows-heatmap-4'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 4000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-4-1.png" alt="plot of chunk tab-top-rows-heatmap-4"/></p>

</div>
<div id='tab-top-rows-heatmap-5'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 5000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-5-1.png" alt="plot of chunk tab-top-rows-heatmap-5"/></p>

</div>
</div>




### Test to known annotations



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.


<script>
$( function() {
	$( '#tabs-test-to-known-factors-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-test-to-known-factors-from-consensus-partition-list'>
<ul>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-test-to-known-factors-from-consensus-partition-list-1'>
<pre><code class="r">test_to_known_factors(res_list, k = 2)
</code></pre>

<pre><code>#&gt;              n disease.state(p) cell.type(p) k
#&gt; SD:NMF      54         2.33e-06     1.17e-03 2
#&gt; CV:NMF      53         9.92e-06     2.43e-03 2
#&gt; MAD:NMF     53         2.77e-05     3.04e-03 2
#&gt; ATC:NMF     54         5.97e-07     6.72e-04 2
#&gt; SD:skmeans  53         6.87e-05     9.70e-03 2
#&gt; CV:skmeans  54         5.02e-04     2.14e-02 2
#&gt; MAD:skmeans 51         2.44e-04     1.83e-02 2
#&gt; ATC:skmeans 53         2.77e-05     3.04e-03 2
#&gt; SD:mclust   54         1.14e-05     4.79e-05 2
#&gt; CV:mclust   53         6.04e-06     2.65e-05 2
#&gt; MAD:mclust  54         4.59e-06     2.04e-05 2
#&gt; ATC:mclust  54         5.52e-05     2.06e-04 2
#&gt; SD:kmeans   54         5.97e-07     6.72e-04 2
#&gt; CV:kmeans   54         5.97e-07     6.72e-04 2
#&gt; MAD:kmeans  54         5.97e-07     6.72e-04 2
#&gt; ATC:kmeans  54         5.97e-07     6.72e-04 2
#&gt; SD:pam      53         2.67e-08     1.83e-04 2
#&gt; CV:pam      53         2.95e-09     6.95e-05 2
#&gt; MAD:pam     53         2.67e-08     1.83e-04 2
#&gt; ATC:pam     54         5.97e-07     6.72e-04 2
#&gt; SD:hclust   54         5.97e-07     6.72e-04 2
#&gt; CV:hclust   52         1.08e-06     8.91e-04 2
#&gt; MAD:hclust  53         8.04e-07     7.74e-04 2
#&gt; ATC:hclust  54         5.97e-07     6.72e-04 2
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-consensus-partition-list-2'>
<pre><code class="r">test_to_known_factors(res_list, k = 3)
</code></pre>

<pre><code>#&gt;              n disease.state(p) cell.type(p) k
#&gt; SD:NMF      53         4.58e-07     6.93e-09 3
#&gt; CV:NMF      52         2.66e-06     1.99e-08 3
#&gt; MAD:NMF     52         2.78e-06     1.35e-07 3
#&gt; ATC:NMF     54         1.50e-07     3.78e-04 3
#&gt; SD:skmeans  51         4.51e-06     3.34e-04 3
#&gt; CV:skmeans  50         6.18e-06     6.36e-04 3
#&gt; MAD:skmeans 51         1.53e-05     5.42e-04 3
#&gt; ATC:skmeans 50         5.25e-05     1.11e-05 3
#&gt; SD:mclust   51         3.42e-09     3.82e-11 3
#&gt; CV:mclust   54         2.57e-07     8.46e-14 3
#&gt; MAD:mclust  54         2.57e-07     8.46e-14 3
#&gt; ATC:mclust  53         1.21e-07     6.60e-08 3
#&gt; SD:kmeans   53         3.58e-07     1.61e-13 3
#&gt; CV:kmeans   54         2.57e-07     8.46e-14 3
#&gt; MAD:kmeans  54         2.57e-07     8.46e-14 3
#&gt; ATC:kmeans  53         3.58e-07     1.61e-13 3
#&gt; SD:pam      52         1.22e-09     2.39e-14 3
#&gt; CV:pam      53         7.99e-10     1.20e-14 3
#&gt; MAD:pam     53         8.86e-09     3.34e-14 3
#&gt; ATC:pam     42         4.32e-06     3.71e-10 3
#&gt; SD:hclust   54         2.57e-07     8.46e-14 3
#&gt; CV:hclust   52         4.99e-07     3.05e-13 3
#&gt; MAD:hclust  53         3.58e-07     1.61e-13 3
#&gt; ATC:hclust  53         1.26e-06     4.07e-04 3
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-consensus-partition-list-3'>
<pre><code class="r">test_to_known_factors(res_list, k = 4)
</code></pre>

<pre><code>#&gt;              n disease.state(p) cell.type(p) k
#&gt; SD:NMF      52         9.58e-08     3.47e-09 4
#&gt; CV:NMF      52         1.25e-08     3.47e-09 4
#&gt; MAD:NMF     49         3.02e-07     9.05e-08 4
#&gt; ATC:NMF     51         1.17e-06     1.72e-09 4
#&gt; SD:skmeans  53         7.85e-06     1.27e-08 4
#&gt; CV:skmeans  47         4.68e-05     2.11e-07 4
#&gt; MAD:skmeans 50         1.91e-05     5.19e-08 4
#&gt; ATC:skmeans 49         8.04e-06     3.00e-05 4
#&gt; SD:mclust   54         3.60e-08     3.28e-14 4
#&gt; CV:mclust   52         4.64e-10     2.91e-14 4
#&gt; MAD:mclust  41         5.82e-07     6.34e-10 4
#&gt; ATC:mclust  53         5.55e-07     2.20e-13 4
#&gt; SD:kmeans   53         1.61e-06     2.73e-12 4
#&gt; CV:kmeans   48         8.02e-06     6.07e-11 4
#&gt; MAD:kmeans  53         1.61e-06     2.73e-12 4
#&gt; ATC:kmeans  51         3.06e-06     9.46e-12 4
#&gt; SD:pam      52         6.37e-09     4.29e-13 4
#&gt; CV:pam      48         3.28e-08     6.15e-12 4
#&gt; MAD:pam     48         2.88e-07     1.52e-11 4
#&gt; ATC:pam     51         8.01e-07     3.06e-12 4
#&gt; SD:hclust   51         7.26e-10     5.96e-14 4
#&gt; CV:hclust   51         7.28e-08     2.59e-13 4
#&gt; MAD:hclust  49         6.39e-09     5.84e-14 4
#&gt; ATC:hclust  54         1.73e-07     1.60e-05 4
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-consensus-partition-list-4'>
<pre><code class="r">test_to_known_factors(res_list, k = 5)
</code></pre>

<pre><code>#&gt;              n disease.state(p) cell.type(p) k
#&gt; SD:NMF      50         7.15e-07     8.69e-09 5
#&gt; CV:NMF      37         2.51e-06     2.20e-06 5
#&gt; MAD:NMF     45         3.77e-06     1.19e-07 5
#&gt; ATC:NMF     48         1.19e-05     2.48e-07 5
#&gt; SD:skmeans  45         8.51e-05     5.35e-07 5
#&gt; CV:skmeans  31         2.28e-03     3.45e-05 5
#&gt; MAD:skmeans 40         1.24e-04     3.51e-06 5
#&gt; ATC:skmeans 42         1.88e-04     7.59e-07 5
#&gt; SD:mclust   44         4.64e-06     3.30e-10 5
#&gt; CV:mclust   39         7.13e-07     7.13e-10 5
#&gt; MAD:mclust  47         1.93e-07     1.29e-10 5
#&gt; ATC:mclust  53         2.03e-06     2.70e-12 5
#&gt; SD:kmeans   54         1.42e-07     4.23e-13 5
#&gt; CV:kmeans   54         1.42e-07     4.23e-13 5
#&gt; MAD:kmeans  54         1.42e-07     4.23e-13 5
#&gt; ATC:kmeans  46         9.79e-05     2.02e-08 5
#&gt; SD:pam      53         1.75e-08     2.70e-12 5
#&gt; CV:pam      47         1.93e-07     1.29e-10 5
#&gt; MAD:pam     50         5.83e-08     1.88e-11 5
#&gt; ATC:pam     34         2.91e-04     7.90e-06 5
#&gt; SD:hclust   53         1.30e-09     1.88e-13 5
#&gt; CV:hclust   49         1.78e-09     2.49e-13 5
#&gt; MAD:hclust  48         5.21e-10     2.84e-14 5
#&gt; ATC:hclust  51         1.07e-06     5.14e-14 5
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-consensus-partition-list-5'>
<pre><code class="r">test_to_known_factors(res_list, k = 6)
</code></pre>

<pre><code>#&gt;              n disease.state(p) cell.type(p) k
#&gt; SD:NMF      48         7.29e-08     3.70e-10 6
#&gt; CV:NMF      47         7.14e-07     1.63e-08 6
#&gt; MAD:NMF     49         2.61e-07     2.78e-08 6
#&gt; ATC:NMF     43         5.46e-07     5.54e-08 6
#&gt; SD:skmeans  30         2.20e-04     4.12e-05 6
#&gt; CV:skmeans  22         7.25e-03     2.15e-03 6
#&gt; MAD:skmeans 27         2.09e-04     7.73e-05 6
#&gt; ATC:skmeans 39         1.83e-04     4.32e-08 6
#&gt; SD:mclust   27         2.37e-03     2.02e-05 6
#&gt; CV:mclust   27         1.05e-04     9.31e-06 6
#&gt; MAD:mclust  42         1.42e-06     3.16e-09 6
#&gt; ATC:mclust  52         6.75e-06     4.64e-11 6
#&gt; SD:kmeans   45         1.12e-05     1.37e-09 6
#&gt; CV:kmeans   49         8.68e-07     1.20e-11 6
#&gt; MAD:kmeans  54         1.42e-07     4.23e-13 6
#&gt; ATC:kmeans  48         1.55e-04     5.11e-08 6
#&gt; SD:pam      49         3.46e-08     1.37e-16 6
#&gt; CV:pam      46         8.86e-07     2.22e-13 6
#&gt; MAD:pam     51         8.68e-08     1.09e-13 6
#&gt; ATC:pam     46         3.94e-06     2.25e-11 6
#&gt; SD:hclust   53         1.30e-09     1.88e-13 6
#&gt; CV:hclust   48         2.78e-09     5.08e-13 6
#&gt; MAD:hclust  45         1.07e-08     4.31e-12 6
#&gt; ATC:hclust  52         1.72e-07     2.44e-14 6
</code></pre>

</div>
</div>


 
## Results for each method


---------------------------------------------------




### SD:hclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "hclust"]
# you can also extract it by
# res = res_list["SD:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21168 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-hclust-collect-plots](figure_cola/SD-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-hclust-select-partition-number](figure_cola/SD-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.980       0.989         0.3753 0.628   0.628
#> 3 3 0.854           0.961       0.960         0.2763 0.874   0.800
#> 4 4 0.944           0.913       0.971         0.1018 0.979   0.958
#> 5 5 0.727           0.914       0.916         0.0829 0.992   0.983
#> 6 6 0.708           0.785       0.811         0.1162 1.000   1.000
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-get-classes'>
<ul>
<li><a href='#tab-SD-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-hclust-get-classes-1'>
<p><a id='tab-SD-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM49613     1  0.0376      0.987 0.996 0.004
#&gt; GSM49604     2  0.1414      0.982 0.020 0.980
#&gt; GSM49605     2  0.0376      0.986 0.004 0.996
#&gt; GSM49606     2  0.0376      0.986 0.004 0.996
#&gt; GSM49607     2  0.0376      0.986 0.004 0.996
#&gt; GSM49608     2  0.0376      0.986 0.004 0.996
#&gt; GSM49609     2  0.0376      0.986 0.004 0.996
#&gt; GSM49610     2  0.0376      0.986 0.004 0.996
#&gt; GSM49611     2  0.0376      0.986 0.004 0.996
#&gt; GSM49612     2  0.0376      0.986 0.004 0.996
#&gt; GSM49614     1  0.0376      0.987 0.996 0.004
#&gt; GSM49615     1  0.0376      0.987 0.996 0.004
#&gt; GSM49616     1  0.0376      0.987 0.996 0.004
#&gt; GSM49617     1  0.0376      0.987 0.996 0.004
#&gt; GSM49564     1  0.0000      0.990 1.000 0.000
#&gt; GSM49565     1  0.0000      0.990 1.000 0.000
#&gt; GSM49566     1  0.0000      0.990 1.000 0.000
#&gt; GSM49567     1  0.0000      0.990 1.000 0.000
#&gt; GSM49568     1  0.0000      0.990 1.000 0.000
#&gt; GSM49569     1  0.0000      0.990 1.000 0.000
#&gt; GSM49570     2  0.2603      0.969 0.044 0.956
#&gt; GSM49571     1  0.7219      0.750 0.800 0.200
#&gt; GSM49572     1  0.0000      0.990 1.000 0.000
#&gt; GSM49573     2  0.2603      0.969 0.044 0.956
#&gt; GSM49574     1  0.0000      0.990 1.000 0.000
#&gt; GSM49575     1  0.0000      0.990 1.000 0.000
#&gt; GSM49576     1  0.0000      0.990 1.000 0.000
#&gt; GSM49577     1  0.0000      0.990 1.000 0.000
#&gt; GSM49578     1  0.0000      0.990 1.000 0.000
#&gt; GSM49579     1  0.0000      0.990 1.000 0.000
#&gt; GSM49580     1  0.0000      0.990 1.000 0.000
#&gt; GSM49581     1  0.0000      0.990 1.000 0.000
#&gt; GSM49582     1  0.0000      0.990 1.000 0.000
#&gt; GSM49583     2  0.1843      0.978 0.028 0.972
#&gt; GSM49584     1  0.0000      0.990 1.000 0.000
#&gt; GSM49585     1  0.0000      0.990 1.000 0.000
#&gt; GSM49586     1  0.6148      0.819 0.848 0.152
#&gt; GSM49587     1  0.0000      0.990 1.000 0.000
#&gt; GSM49588     1  0.0000      0.990 1.000 0.000
#&gt; GSM49589     1  0.0000      0.990 1.000 0.000
#&gt; GSM49590     1  0.0000      0.990 1.000 0.000
#&gt; GSM49591     1  0.0000      0.990 1.000 0.000
#&gt; GSM49592     1  0.0000      0.990 1.000 0.000
#&gt; GSM49593     1  0.0000      0.990 1.000 0.000
#&gt; GSM49594     1  0.0000      0.990 1.000 0.000
#&gt; GSM49595     1  0.0000      0.990 1.000 0.000
#&gt; GSM49596     1  0.0000      0.990 1.000 0.000
#&gt; GSM49597     2  0.2603      0.969 0.044 0.956
#&gt; GSM49598     1  0.0000      0.990 1.000 0.000
#&gt; GSM49599     1  0.0000      0.990 1.000 0.000
#&gt; GSM49600     1  0.0000      0.990 1.000 0.000
#&gt; GSM49601     1  0.0000      0.990 1.000 0.000
#&gt; GSM49602     1  0.0000      0.990 1.000 0.000
#&gt; GSM49603     1  0.0000      0.990 1.000 0.000
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-1-a').click(function(){
  $('#tab-SD-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-2'>
<p><a id='tab-SD-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM49613     3  0.4702      1.000 0.212 0.000 0.788
#&gt; GSM49604     2  0.3141      0.914 0.020 0.912 0.068
#&gt; GSM49605     2  0.0000      0.946 0.000 1.000 0.000
#&gt; GSM49606     2  0.0000      0.946 0.000 1.000 0.000
#&gt; GSM49607     2  0.0000      0.946 0.000 1.000 0.000
#&gt; GSM49608     2  0.0000      0.946 0.000 1.000 0.000
#&gt; GSM49609     2  0.0000      0.946 0.000 1.000 0.000
#&gt; GSM49610     2  0.0000      0.946 0.000 1.000 0.000
#&gt; GSM49611     2  0.0000      0.946 0.000 1.000 0.000
#&gt; GSM49612     2  0.0000      0.946 0.000 1.000 0.000
#&gt; GSM49614     3  0.4702      1.000 0.212 0.000 0.788
#&gt; GSM49615     3  0.4702      1.000 0.212 0.000 0.788
#&gt; GSM49616     3  0.4702      1.000 0.212 0.000 0.788
#&gt; GSM49617     3  0.4702      1.000 0.212 0.000 0.788
#&gt; GSM49564     1  0.0000      0.986 1.000 0.000 0.000
#&gt; GSM49565     1  0.0000      0.986 1.000 0.000 0.000
#&gt; GSM49566     1  0.0000      0.986 1.000 0.000 0.000
#&gt; GSM49567     1  0.0000      0.986 1.000 0.000 0.000
#&gt; GSM49568     1  0.0000      0.986 1.000 0.000 0.000
#&gt; GSM49569     1  0.0000      0.986 1.000 0.000 0.000
#&gt; GSM49570     2  0.6247      0.807 0.044 0.744 0.212
#&gt; GSM49571     1  0.5514      0.683 0.800 0.156 0.044
#&gt; GSM49572     1  0.0000      0.986 1.000 0.000 0.000
#&gt; GSM49573     2  0.6247      0.807 0.044 0.744 0.212
#&gt; GSM49574     1  0.0000      0.986 1.000 0.000 0.000
#&gt; GSM49575     1  0.0000      0.986 1.000 0.000 0.000
#&gt; GSM49576     1  0.0000      0.986 1.000 0.000 0.000
#&gt; GSM49577     1  0.0000      0.986 1.000 0.000 0.000
#&gt; GSM49578     1  0.0000      0.986 1.000 0.000 0.000
#&gt; GSM49579     1  0.0000      0.986 1.000 0.000 0.000
#&gt; GSM49580     1  0.0000      0.986 1.000 0.000 0.000
#&gt; GSM49581     1  0.0000      0.986 1.000 0.000 0.000
#&gt; GSM49582     1  0.0000      0.986 1.000 0.000 0.000
#&gt; GSM49583     2  0.2681      0.921 0.028 0.932 0.040
#&gt; GSM49584     1  0.0000      0.986 1.000 0.000 0.000
#&gt; GSM49585     1  0.0000      0.986 1.000 0.000 0.000
#&gt; GSM49586     1  0.4676      0.770 0.848 0.112 0.040
#&gt; GSM49587     1  0.0000      0.986 1.000 0.000 0.000
#&gt; GSM49588     1  0.0000      0.986 1.000 0.000 0.000
#&gt; GSM49589     1  0.0000      0.986 1.000 0.000 0.000
#&gt; GSM49590     1  0.0237      0.982 0.996 0.000 0.004
#&gt; GSM49591     1  0.0000      0.986 1.000 0.000 0.000
#&gt; GSM49592     1  0.0000      0.986 1.000 0.000 0.000
#&gt; GSM49593     1  0.0000      0.986 1.000 0.000 0.000
#&gt; GSM49594     1  0.0000      0.986 1.000 0.000 0.000
#&gt; GSM49595     1  0.0000      0.986 1.000 0.000 0.000
#&gt; GSM49596     1  0.0000      0.986 1.000 0.000 0.000
#&gt; GSM49597     2  0.3267      0.904 0.044 0.912 0.044
#&gt; GSM49598     1  0.0000      0.986 1.000 0.000 0.000
#&gt; GSM49599     1  0.0000      0.986 1.000 0.000 0.000
#&gt; GSM49600     1  0.0000      0.986 1.000 0.000 0.000
#&gt; GSM49601     1  0.0000      0.986 1.000 0.000 0.000
#&gt; GSM49602     1  0.0000      0.986 1.000 0.000 0.000
#&gt; GSM49603     1  0.0000      0.986 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-2-a').click(function(){
  $('#tab-SD-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-3'>
<p><a id='tab-SD-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM49613     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM49604     2  0.4454      0.451 0.000 0.692 0.000 0.308
#&gt; GSM49605     2  0.0000      0.903 0.000 1.000 0.000 0.000
#&gt; GSM49606     2  0.0000      0.903 0.000 1.000 0.000 0.000
#&gt; GSM49607     2  0.0000      0.903 0.000 1.000 0.000 0.000
#&gt; GSM49608     2  0.0000      0.903 0.000 1.000 0.000 0.000
#&gt; GSM49609     2  0.0000      0.903 0.000 1.000 0.000 0.000
#&gt; GSM49610     2  0.0000      0.903 0.000 1.000 0.000 0.000
#&gt; GSM49611     2  0.0000      0.903 0.000 1.000 0.000 0.000
#&gt; GSM49612     2  0.0000      0.903 0.000 1.000 0.000 0.000
#&gt; GSM49614     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM49615     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM49616     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM49617     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM49564     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49565     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49566     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49567     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49568     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49569     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49570     4  0.0000      0.684 0.000 0.000 0.000 1.000
#&gt; GSM49571     1  0.3764      0.733 0.784 0.000 0.000 0.216
#&gt; GSM49572     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49573     4  0.0000      0.684 0.000 0.000 0.000 1.000
#&gt; GSM49574     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49575     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49576     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49577     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49578     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49579     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49580     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49581     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49582     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49583     2  0.4761      0.226 0.000 0.628 0.000 0.372
#&gt; GSM49584     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49585     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49586     1  0.3123      0.818 0.844 0.000 0.000 0.156
#&gt; GSM49587     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49588     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49589     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49590     1  0.0524      0.978 0.988 0.000 0.004 0.008
#&gt; GSM49591     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49592     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49593     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49594     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49595     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49596     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49597     4  0.5000     -0.139 0.000 0.496 0.000 0.504
#&gt; GSM49598     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49599     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49600     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49601     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49602     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49603     1  0.0000      0.989 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-3-a').click(function(){
  $('#tab-SD-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-4'>
<p><a id='tab-SD-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM49613     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM49604     2  0.4983      0.475 0.000 0.664 0.000 0.272 0.064
#&gt; GSM49605     2  0.0000      0.952 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49606     2  0.0000      0.952 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49607     2  0.0000      0.952 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49608     2  0.0000      0.952 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49609     2  0.0000      0.952 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49610     2  0.0000      0.952 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49611     2  0.0000      0.952 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49612     2  0.0000      0.952 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49614     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM49615     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM49616     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM49617     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM49564     1  0.1908      0.916 0.908 0.000 0.000 0.000 0.092
#&gt; GSM49565     1  0.1608      0.925 0.928 0.000 0.000 0.000 0.072
#&gt; GSM49566     1  0.1908      0.916 0.908 0.000 0.000 0.000 0.092
#&gt; GSM49567     1  0.1410      0.927 0.940 0.000 0.000 0.000 0.060
#&gt; GSM49568     1  0.1478      0.919 0.936 0.000 0.000 0.000 0.064
#&gt; GSM49569     1  0.1608      0.923 0.928 0.000 0.000 0.000 0.072
#&gt; GSM49570     4  0.0162      0.994 0.000 0.000 0.000 0.996 0.004
#&gt; GSM49571     1  0.4234      0.746 0.760 0.000 0.000 0.184 0.056
#&gt; GSM49572     1  0.1410      0.920 0.940 0.000 0.000 0.000 0.060
#&gt; GSM49573     4  0.0000      0.994 0.000 0.000 0.000 1.000 0.000
#&gt; GSM49574     1  0.1851      0.909 0.912 0.000 0.000 0.000 0.088
#&gt; GSM49575     1  0.1851      0.909 0.912 0.000 0.000 0.000 0.088
#&gt; GSM49576     1  0.1732      0.920 0.920 0.000 0.000 0.000 0.080
#&gt; GSM49577     1  0.1608      0.926 0.928 0.000 0.000 0.000 0.072
#&gt; GSM49578     1  0.1851      0.909 0.912 0.000 0.000 0.000 0.088
#&gt; GSM49579     1  0.1851      0.921 0.912 0.000 0.000 0.000 0.088
#&gt; GSM49580     1  0.1043      0.924 0.960 0.000 0.000 0.000 0.040
#&gt; GSM49581     1  0.1410      0.919 0.940 0.000 0.000 0.000 0.060
#&gt; GSM49582     1  0.1851      0.909 0.912 0.000 0.000 0.000 0.088
#&gt; GSM49583     5  0.4849      0.744 0.000 0.140 0.000 0.136 0.724
#&gt; GSM49584     1  0.1043      0.924 0.960 0.000 0.000 0.000 0.040
#&gt; GSM49585     1  0.1544      0.918 0.932 0.000 0.000 0.000 0.068
#&gt; GSM49586     1  0.3814      0.804 0.808 0.000 0.000 0.124 0.068
#&gt; GSM49587     1  0.1732      0.913 0.920 0.000 0.000 0.000 0.080
#&gt; GSM49588     1  0.1544      0.927 0.932 0.000 0.000 0.000 0.068
#&gt; GSM49589     1  0.1908      0.916 0.908 0.000 0.000 0.000 0.092
#&gt; GSM49590     1  0.2233      0.909 0.892 0.000 0.004 0.000 0.104
#&gt; GSM49591     1  0.1732      0.913 0.920 0.000 0.000 0.000 0.080
#&gt; GSM49592     1  0.1851      0.909 0.912 0.000 0.000 0.000 0.088
#&gt; GSM49593     1  0.1732      0.920 0.920 0.000 0.000 0.000 0.080
#&gt; GSM49594     1  0.1608      0.923 0.928 0.000 0.000 0.000 0.072
#&gt; GSM49595     1  0.1608      0.923 0.928 0.000 0.000 0.000 0.072
#&gt; GSM49596     1  0.1732      0.920 0.920 0.000 0.000 0.000 0.080
#&gt; GSM49597     5  0.3318      0.727 0.000 0.008 0.000 0.192 0.800
#&gt; GSM49598     1  0.1732      0.920 0.920 0.000 0.000 0.000 0.080
#&gt; GSM49599     1  0.1043      0.925 0.960 0.000 0.000 0.000 0.040
#&gt; GSM49600     1  0.0963      0.927 0.964 0.000 0.000 0.000 0.036
#&gt; GSM49601     1  0.1792      0.919 0.916 0.000 0.000 0.000 0.084
#&gt; GSM49602     1  0.1792      0.919 0.916 0.000 0.000 0.000 0.084
#&gt; GSM49603     1  0.1792      0.919 0.916 0.000 0.000 0.000 0.084
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-4-a').click(function(){
  $('#tab-SD-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-5'>
<p><a id='tab-SD-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2 p3    p4    p5 p6
#&gt; GSM49613     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 NA
#&gt; GSM49604     2  0.5993      0.290 0.000 0.608  0 0.148 0.068 NA
#&gt; GSM49605     2  0.3428      0.757 0.000 0.696  0 0.000 0.000 NA
#&gt; GSM49606     2  0.3428      0.757 0.000 0.696  0 0.000 0.000 NA
#&gt; GSM49607     2  0.3428      0.757 0.000 0.696  0 0.000 0.000 NA
#&gt; GSM49608     2  0.3428      0.757 0.000 0.696  0 0.000 0.000 NA
#&gt; GSM49609     2  0.0000      0.776 0.000 1.000  0 0.000 0.000 NA
#&gt; GSM49610     2  0.0000      0.776 0.000 1.000  0 0.000 0.000 NA
#&gt; GSM49611     2  0.0000      0.776 0.000 1.000  0 0.000 0.000 NA
#&gt; GSM49612     2  0.0000      0.776 0.000 1.000  0 0.000 0.000 NA
#&gt; GSM49614     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 NA
#&gt; GSM49615     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 NA
#&gt; GSM49616     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 NA
#&gt; GSM49617     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 NA
#&gt; GSM49564     1  0.3823      0.710 0.564 0.000  0 0.000 0.000 NA
#&gt; GSM49565     1  0.1327      0.806 0.936 0.000  0 0.000 0.000 NA
#&gt; GSM49566     1  0.3817      0.713 0.568 0.000  0 0.000 0.000 NA
#&gt; GSM49567     1  0.1267      0.808 0.940 0.000  0 0.000 0.000 NA
#&gt; GSM49568     1  0.0713      0.798 0.972 0.000  0 0.000 0.000 NA
#&gt; GSM49569     1  0.3592      0.761 0.656 0.000  0 0.000 0.000 NA
#&gt; GSM49570     4  0.0713      0.969 0.000 0.000  0 0.972 0.028 NA
#&gt; GSM49571     1  0.4862      0.570 0.720 0.000  0 0.060 0.064 NA
#&gt; GSM49572     1  0.0632      0.799 0.976 0.000  0 0.000 0.000 NA
#&gt; GSM49573     4  0.0000      0.969 0.000 0.000  0 1.000 0.000 NA
#&gt; GSM49574     1  0.0547      0.787 0.980 0.000  0 0.000 0.000 NA
#&gt; GSM49575     1  0.1501      0.752 0.924 0.000  0 0.000 0.000 NA
#&gt; GSM49576     1  0.3659      0.753 0.636 0.000  0 0.000 0.000 NA
#&gt; GSM49577     1  0.2219      0.805 0.864 0.000  0 0.000 0.000 NA
#&gt; GSM49578     1  0.0547      0.787 0.980 0.000  0 0.000 0.000 NA
#&gt; GSM49579     1  0.3647      0.754 0.640 0.000  0 0.000 0.000 NA
#&gt; GSM49580     1  0.1007      0.806 0.956 0.000  0 0.000 0.000 NA
#&gt; GSM49581     1  0.0713      0.803 0.972 0.000  0 0.000 0.000 NA
#&gt; GSM49582     1  0.0547      0.787 0.980 0.000  0 0.000 0.000 NA
#&gt; GSM49583     5  0.3073      0.820 0.000 0.080  0 0.000 0.840 NA
#&gt; GSM49584     1  0.0865      0.804 0.964 0.000  0 0.000 0.000 NA
#&gt; GSM49585     1  0.0937      0.804 0.960 0.000  0 0.000 0.000 NA
#&gt; GSM49586     1  0.4205      0.630 0.744 0.000  0 0.012 0.060 NA
#&gt; GSM49587     1  0.0363      0.791 0.988 0.000  0 0.000 0.000 NA
#&gt; GSM49588     1  0.1556      0.809 0.920 0.000  0 0.000 0.000 NA
#&gt; GSM49589     1  0.3817      0.713 0.568 0.000  0 0.000 0.000 NA
#&gt; GSM49590     1  0.4067      0.698 0.548 0.000  0 0.000 0.008 NA
#&gt; GSM49591     1  0.0458      0.793 0.984 0.000  0 0.000 0.000 NA
#&gt; GSM49592     1  0.0547      0.787 0.980 0.000  0 0.000 0.000 NA
#&gt; GSM49593     1  0.3659      0.753 0.636 0.000  0 0.000 0.000 NA
#&gt; GSM49594     1  0.3684      0.750 0.628 0.000  0 0.000 0.000 NA
#&gt; GSM49595     1  0.3684      0.750 0.628 0.000  0 0.000 0.000 NA
#&gt; GSM49596     1  0.3659      0.753 0.636 0.000  0 0.000 0.000 NA
#&gt; GSM49597     5  0.0000      0.817 0.000 0.000  0 0.000 1.000 NA
#&gt; GSM49598     1  0.3244      0.782 0.732 0.000  0 0.000 0.000 NA
#&gt; GSM49599     1  0.0865      0.806 0.964 0.000  0 0.000 0.000 NA
#&gt; GSM49600     1  0.2135      0.806 0.872 0.000  0 0.000 0.000 NA
#&gt; GSM49601     1  0.3797      0.721 0.580 0.000  0 0.000 0.000 NA
#&gt; GSM49602     1  0.3797      0.721 0.580 0.000  0 0.000 0.000 NA
#&gt; GSM49603     1  0.3797      0.721 0.580 0.000  0 0.000 0.000 NA
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-5-a').click(function(){
  $('#tab-SD-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-SD-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-get-signatures'>
<ul>
<li><a href='#tab-SD-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-1-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-1"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-2-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-2"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-3-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-3"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-4-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-4"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-5-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-hclust-signature_compare](figure_cola/SD-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-SD-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-hclust-collect-classes](figure_cola/SD-hclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n disease.state(p) cell.type(p) k
#> SD:hclust 54         5.97e-07     6.72e-04 2
#> SD:hclust 54         2.57e-07     8.46e-14 3
#> SD:hclust 51         7.26e-10     5.96e-14 4
#> SD:hclust 53         1.30e-09     1.88e-13 5
#> SD:hclust 53         1.30e-09     1.88e-13 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:kmeans






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "kmeans"]
# you can also extract it by
# res = res_list["SD:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21168 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-kmeans-collect-plots](figure_cola/SD-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-kmeans-select-partition-number](figure_cola/SD-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.543           0.953       0.940         0.3452 0.628   0.628
#> 3 3 0.859           0.961       0.968         0.4485 0.874   0.800
#> 4 4 0.746           0.878       0.719         0.2691 0.774   0.550
#> 5 5 0.709           0.947       0.902         0.1606 0.959   0.855
#> 6 6 0.788           0.738       0.768         0.0781 0.951   0.797
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-get-classes'>
<ul>
<li><a href='#tab-SD-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-kmeans-get-classes-1'>
<p><a id='tab-SD-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM49613     1   0.644      0.810 0.836 0.164
#&gt; GSM49604     2   0.644      1.000 0.164 0.836
#&gt; GSM49605     2   0.644      1.000 0.164 0.836
#&gt; GSM49606     2   0.644      1.000 0.164 0.836
#&gt; GSM49607     2   0.644      1.000 0.164 0.836
#&gt; GSM49608     2   0.644      1.000 0.164 0.836
#&gt; GSM49609     2   0.644      1.000 0.164 0.836
#&gt; GSM49610     2   0.644      1.000 0.164 0.836
#&gt; GSM49611     2   0.644      1.000 0.164 0.836
#&gt; GSM49612     2   0.644      1.000 0.164 0.836
#&gt; GSM49614     1   0.644      0.810 0.836 0.164
#&gt; GSM49615     1   0.644      0.810 0.836 0.164
#&gt; GSM49616     1   0.644      0.810 0.836 0.164
#&gt; GSM49617     1   0.644      0.810 0.836 0.164
#&gt; GSM49564     1   0.000      0.967 1.000 0.000
#&gt; GSM49565     1   0.000      0.967 1.000 0.000
#&gt; GSM49566     1   0.000      0.967 1.000 0.000
#&gt; GSM49567     1   0.000      0.967 1.000 0.000
#&gt; GSM49568     1   0.000      0.967 1.000 0.000
#&gt; GSM49569     1   0.000      0.967 1.000 0.000
#&gt; GSM49570     2   0.644      1.000 0.164 0.836
#&gt; GSM49571     1   0.833      0.538 0.736 0.264
#&gt; GSM49572     1   0.000      0.967 1.000 0.000
#&gt; GSM49573     2   0.644      1.000 0.164 0.836
#&gt; GSM49574     1   0.000      0.967 1.000 0.000
#&gt; GSM49575     1   0.000      0.967 1.000 0.000
#&gt; GSM49576     1   0.000      0.967 1.000 0.000
#&gt; GSM49577     1   0.000      0.967 1.000 0.000
#&gt; GSM49578     1   0.000      0.967 1.000 0.000
#&gt; GSM49579     1   0.000      0.967 1.000 0.000
#&gt; GSM49580     1   0.000      0.967 1.000 0.000
#&gt; GSM49581     1   0.000      0.967 1.000 0.000
#&gt; GSM49582     1   0.000      0.967 1.000 0.000
#&gt; GSM49583     2   0.644      1.000 0.164 0.836
#&gt; GSM49584     1   0.000      0.967 1.000 0.000
#&gt; GSM49585     1   0.000      0.967 1.000 0.000
#&gt; GSM49586     1   0.000      0.967 1.000 0.000
#&gt; GSM49587     1   0.000      0.967 1.000 0.000
#&gt; GSM49588     1   0.000      0.967 1.000 0.000
#&gt; GSM49589     1   0.000      0.967 1.000 0.000
#&gt; GSM49590     1   0.000      0.967 1.000 0.000
#&gt; GSM49591     1   0.000      0.967 1.000 0.000
#&gt; GSM49592     1   0.000      0.967 1.000 0.000
#&gt; GSM49593     1   0.000      0.967 1.000 0.000
#&gt; GSM49594     1   0.000      0.967 1.000 0.000
#&gt; GSM49595     1   0.000      0.967 1.000 0.000
#&gt; GSM49596     1   0.000      0.967 1.000 0.000
#&gt; GSM49597     2   0.644      1.000 0.164 0.836
#&gt; GSM49598     1   0.000      0.967 1.000 0.000
#&gt; GSM49599     1   0.000      0.967 1.000 0.000
#&gt; GSM49600     1   0.000      0.967 1.000 0.000
#&gt; GSM49601     1   0.000      0.967 1.000 0.000
#&gt; GSM49602     1   0.000      0.967 1.000 0.000
#&gt; GSM49603     1   0.000      0.967 1.000 0.000
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-1-a').click(function(){
  $('#tab-SD-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-2'>
<p><a id='tab-SD-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM49613     3  0.2959      1.000 0.100 0.000 0.900
#&gt; GSM49604     2  0.2796      0.936 0.000 0.908 0.092
#&gt; GSM49605     2  0.0424      0.970 0.000 0.992 0.008
#&gt; GSM49606     2  0.0424      0.970 0.000 0.992 0.008
#&gt; GSM49607     2  0.0424      0.970 0.000 0.992 0.008
#&gt; GSM49608     2  0.0424      0.970 0.000 0.992 0.008
#&gt; GSM49609     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM49610     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM49611     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM49612     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM49614     3  0.2959      1.000 0.100 0.000 0.900
#&gt; GSM49615     3  0.2959      1.000 0.100 0.000 0.900
#&gt; GSM49616     3  0.2959      1.000 0.100 0.000 0.900
#&gt; GSM49617     3  0.2959      1.000 0.100 0.000 0.900
#&gt; GSM49564     1  0.1289      0.968 0.968 0.000 0.032
#&gt; GSM49565     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM49566     1  0.1031      0.969 0.976 0.000 0.024
#&gt; GSM49567     1  0.0237      0.974 0.996 0.000 0.004
#&gt; GSM49568     1  0.0237      0.974 0.996 0.000 0.004
#&gt; GSM49569     1  0.1163      0.968 0.972 0.000 0.028
#&gt; GSM49570     2  0.2796      0.936 0.000 0.908 0.092
#&gt; GSM49571     1  0.7848      0.429 0.640 0.264 0.096
#&gt; GSM49572     1  0.0237      0.974 0.996 0.000 0.004
#&gt; GSM49573     2  0.2796      0.936 0.000 0.908 0.092
#&gt; GSM49574     1  0.0237      0.974 0.996 0.000 0.004
#&gt; GSM49575     1  0.0237      0.974 0.996 0.000 0.004
#&gt; GSM49576     1  0.1163      0.968 0.972 0.000 0.028
#&gt; GSM49577     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM49578     1  0.0237      0.974 0.996 0.000 0.004
#&gt; GSM49579     1  0.0747      0.971 0.984 0.000 0.016
#&gt; GSM49580     1  0.0237      0.974 0.996 0.000 0.004
#&gt; GSM49581     1  0.0237      0.974 0.996 0.000 0.004
#&gt; GSM49582     1  0.0237      0.974 0.996 0.000 0.004
#&gt; GSM49583     2  0.0424      0.970 0.000 0.992 0.008
#&gt; GSM49584     1  0.0237      0.974 0.996 0.000 0.004
#&gt; GSM49585     1  0.0237      0.974 0.996 0.000 0.004
#&gt; GSM49586     1  0.1163      0.968 0.972 0.000 0.028
#&gt; GSM49587     1  0.0237      0.974 0.996 0.000 0.004
#&gt; GSM49588     1  0.0237      0.974 0.996 0.000 0.004
#&gt; GSM49589     1  0.1163      0.968 0.972 0.000 0.028
#&gt; GSM49590     1  0.1163      0.968 0.972 0.000 0.028
#&gt; GSM49591     1  0.0237      0.974 0.996 0.000 0.004
#&gt; GSM49592     1  0.0237      0.974 0.996 0.000 0.004
#&gt; GSM49593     1  0.1163      0.968 0.972 0.000 0.028
#&gt; GSM49594     1  0.1163      0.968 0.972 0.000 0.028
#&gt; GSM49595     1  0.1163      0.968 0.972 0.000 0.028
#&gt; GSM49596     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM49597     2  0.2959      0.936 0.000 0.900 0.100
#&gt; GSM49598     1  0.1163      0.968 0.972 0.000 0.028
#&gt; GSM49599     1  0.0237      0.974 0.996 0.000 0.004
#&gt; GSM49600     1  0.0237      0.974 0.996 0.000 0.004
#&gt; GSM49601     1  0.1163      0.968 0.972 0.000 0.028
#&gt; GSM49602     1  0.1163      0.968 0.972 0.000 0.028
#&gt; GSM49603     1  0.1163      0.968 0.972 0.000 0.028
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-2-a').click(function(){
  $('#tab-SD-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-3'>
<p><a id='tab-SD-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM49613     3  0.1706      0.994 0.016 0.000 0.948 0.036
#&gt; GSM49604     2  0.5503      0.611 0.468 0.516 0.016 0.000
#&gt; GSM49605     2  0.0188      0.837 0.000 0.996 0.004 0.000
#&gt; GSM49606     2  0.0000      0.837 0.000 1.000 0.000 0.000
#&gt; GSM49607     2  0.0188      0.837 0.000 0.996 0.004 0.000
#&gt; GSM49608     2  0.0188      0.837 0.000 0.996 0.004 0.000
#&gt; GSM49609     2  0.1042      0.836 0.008 0.972 0.020 0.000
#&gt; GSM49610     2  0.1042      0.836 0.008 0.972 0.020 0.000
#&gt; GSM49611     2  0.1042      0.836 0.008 0.972 0.020 0.000
#&gt; GSM49612     2  0.1042      0.836 0.008 0.972 0.020 0.000
#&gt; GSM49614     3  0.1452      0.995 0.008 0.000 0.956 0.036
#&gt; GSM49615     3  0.1706      0.994 0.016 0.000 0.948 0.036
#&gt; GSM49616     3  0.1305      0.995 0.004 0.000 0.960 0.036
#&gt; GSM49617     3  0.1452      0.995 0.008 0.000 0.956 0.036
#&gt; GSM49564     4  0.0469      0.973 0.012 0.000 0.000 0.988
#&gt; GSM49565     1  0.4998      0.896 0.512 0.000 0.000 0.488
#&gt; GSM49566     4  0.0336      0.980 0.008 0.000 0.000 0.992
#&gt; GSM49567     1  0.4996      0.901 0.516 0.000 0.000 0.484
#&gt; GSM49568     1  0.4999      0.912 0.508 0.000 0.000 0.492
#&gt; GSM49569     4  0.0000      0.984 0.000 0.000 0.000 1.000
#&gt; GSM49570     2  0.5914      0.603 0.468 0.504 0.016 0.012
#&gt; GSM49571     1  0.7928     -0.440 0.468 0.184 0.016 0.332
#&gt; GSM49572     1  0.4996      0.901 0.516 0.000 0.000 0.484
#&gt; GSM49573     2  0.5914      0.603 0.468 0.504 0.016 0.012
#&gt; GSM49574     1  0.4999      0.912 0.508 0.000 0.000 0.492
#&gt; GSM49575     1  0.4999      0.912 0.508 0.000 0.000 0.492
#&gt; GSM49576     4  0.0336      0.980 0.008 0.000 0.000 0.992
#&gt; GSM49577     4  0.1716      0.887 0.064 0.000 0.000 0.936
#&gt; GSM49578     1  0.4999      0.912 0.508 0.000 0.000 0.492
#&gt; GSM49579     4  0.0707      0.970 0.020 0.000 0.000 0.980
#&gt; GSM49580     1  0.4999      0.912 0.508 0.000 0.000 0.492
#&gt; GSM49581     1  0.4999      0.912 0.508 0.000 0.000 0.492
#&gt; GSM49582     1  0.4999      0.912 0.508 0.000 0.000 0.492
#&gt; GSM49583     2  0.0188      0.837 0.000 0.996 0.004 0.000
#&gt; GSM49584     1  0.4999      0.912 0.508 0.000 0.000 0.492
#&gt; GSM49585     1  0.4999      0.912 0.508 0.000 0.000 0.492
#&gt; GSM49586     4  0.0000      0.984 0.000 0.000 0.000 1.000
#&gt; GSM49587     1  0.4999      0.912 0.508 0.000 0.000 0.492
#&gt; GSM49588     1  0.4999      0.912 0.508 0.000 0.000 0.492
#&gt; GSM49589     4  0.0000      0.984 0.000 0.000 0.000 1.000
#&gt; GSM49590     4  0.0336      0.980 0.008 0.000 0.000 0.992
#&gt; GSM49591     1  0.4999      0.912 0.508 0.000 0.000 0.492
#&gt; GSM49592     1  0.4999      0.912 0.508 0.000 0.000 0.492
#&gt; GSM49593     4  0.0000      0.984 0.000 0.000 0.000 1.000
#&gt; GSM49594     4  0.0336      0.980 0.008 0.000 0.000 0.992
#&gt; GSM49595     4  0.0336      0.980 0.008 0.000 0.000 0.992
#&gt; GSM49596     4  0.0592      0.963 0.016 0.000 0.000 0.984
#&gt; GSM49597     2  0.5995      0.608 0.448 0.520 0.020 0.012
#&gt; GSM49598     4  0.0000      0.984 0.000 0.000 0.000 1.000
#&gt; GSM49599     1  0.4999      0.912 0.508 0.000 0.000 0.492
#&gt; GSM49600     1  0.5000      0.905 0.504 0.000 0.000 0.496
#&gt; GSM49601     4  0.0000      0.984 0.000 0.000 0.000 1.000
#&gt; GSM49602     4  0.0000      0.984 0.000 0.000 0.000 1.000
#&gt; GSM49603     4  0.0000      0.984 0.000 0.000 0.000 1.000
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-3-a').click(function(){
  $('#tab-SD-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-4'>
<p><a id='tab-SD-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM49613     3  0.0579      0.990 0.008 0.000 0.984 0.008 0.000
#&gt; GSM49604     4  0.2966      0.917 0.000 0.184 0.000 0.816 0.000
#&gt; GSM49605     2  0.0963      0.942 0.036 0.964 0.000 0.000 0.000
#&gt; GSM49606     2  0.0000      0.945 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49607     2  0.1121      0.938 0.044 0.956 0.000 0.000 0.000
#&gt; GSM49608     2  0.0963      0.942 0.036 0.964 0.000 0.000 0.000
#&gt; GSM49609     2  0.1628      0.939 0.056 0.936 0.000 0.008 0.000
#&gt; GSM49610     2  0.1628      0.939 0.056 0.936 0.000 0.008 0.000
#&gt; GSM49611     2  0.1628      0.939 0.056 0.936 0.000 0.008 0.000
#&gt; GSM49612     2  0.1628      0.939 0.056 0.936 0.000 0.008 0.000
#&gt; GSM49614     3  0.0162      0.992 0.000 0.000 0.996 0.004 0.000
#&gt; GSM49615     3  0.0579      0.990 0.008 0.000 0.984 0.008 0.000
#&gt; GSM49616     3  0.0000      0.993 0.000 0.000 1.000 0.000 0.000
#&gt; GSM49617     3  0.0162      0.992 0.000 0.000 0.996 0.004 0.000
#&gt; GSM49564     5  0.1549      0.952 0.016 0.000 0.000 0.040 0.944
#&gt; GSM49565     1  0.4291      0.942 0.772 0.000 0.000 0.092 0.136
#&gt; GSM49566     5  0.1740      0.939 0.012 0.000 0.000 0.056 0.932
#&gt; GSM49567     1  0.3551      0.942 0.820 0.000 0.000 0.044 0.136
#&gt; GSM49568     1  0.2753      0.959 0.856 0.000 0.000 0.008 0.136
#&gt; GSM49569     5  0.0807      0.962 0.012 0.000 0.000 0.012 0.976
#&gt; GSM49570     4  0.3355      0.916 0.012 0.184 0.000 0.804 0.000
#&gt; GSM49571     4  0.3047      0.771 0.004 0.044 0.000 0.868 0.084
#&gt; GSM49572     1  0.4238      0.944 0.776 0.000 0.000 0.088 0.136
#&gt; GSM49573     4  0.2966      0.917 0.000 0.184 0.000 0.816 0.000
#&gt; GSM49574     1  0.3966      0.957 0.796 0.000 0.000 0.072 0.132
#&gt; GSM49575     1  0.2818      0.957 0.856 0.000 0.000 0.012 0.132
#&gt; GSM49576     5  0.0290      0.962 0.008 0.000 0.000 0.000 0.992
#&gt; GSM49577     5  0.2922      0.877 0.056 0.000 0.000 0.072 0.872
#&gt; GSM49578     1  0.2864      0.960 0.852 0.000 0.000 0.012 0.136
#&gt; GSM49579     5  0.1195      0.953 0.012 0.000 0.000 0.028 0.960
#&gt; GSM49580     1  0.3151      0.951 0.836 0.000 0.000 0.020 0.144
#&gt; GSM49581     1  0.2864      0.958 0.852 0.000 0.000 0.012 0.136
#&gt; GSM49582     1  0.2753      0.959 0.856 0.000 0.000 0.008 0.136
#&gt; GSM49583     2  0.1197      0.936 0.048 0.952 0.000 0.000 0.000
#&gt; GSM49584     1  0.2753      0.959 0.856 0.000 0.000 0.008 0.136
#&gt; GSM49585     1  0.3932      0.956 0.796 0.000 0.000 0.064 0.140
#&gt; GSM49586     5  0.1701      0.951 0.016 0.000 0.000 0.048 0.936
#&gt; GSM49587     1  0.3825      0.958 0.804 0.000 0.000 0.060 0.136
#&gt; GSM49588     1  0.3932      0.956 0.796 0.000 0.000 0.064 0.140
#&gt; GSM49589     5  0.1430      0.960 0.004 0.000 0.000 0.052 0.944
#&gt; GSM49590     5  0.0162      0.963 0.004 0.000 0.000 0.000 0.996
#&gt; GSM49591     1  0.3906      0.956 0.800 0.000 0.000 0.068 0.132
#&gt; GSM49592     1  0.3950      0.958 0.796 0.000 0.000 0.068 0.136
#&gt; GSM49593     5  0.0404      0.963 0.012 0.000 0.000 0.000 0.988
#&gt; GSM49594     5  0.1357      0.955 0.004 0.000 0.000 0.048 0.948
#&gt; GSM49595     5  0.1357      0.955 0.004 0.000 0.000 0.048 0.948
#&gt; GSM49596     5  0.0807      0.962 0.012 0.000 0.000 0.012 0.976
#&gt; GSM49597     4  0.4617      0.856 0.060 0.224 0.000 0.716 0.000
#&gt; GSM49598     5  0.0404      0.963 0.012 0.000 0.000 0.000 0.988
#&gt; GSM49599     1  0.3506      0.958 0.824 0.000 0.000 0.044 0.132
#&gt; GSM49600     1  0.3359      0.939 0.816 0.000 0.000 0.020 0.164
#&gt; GSM49601     5  0.1522      0.958 0.012 0.000 0.000 0.044 0.944
#&gt; GSM49602     5  0.1364      0.960 0.012 0.000 0.000 0.036 0.952
#&gt; GSM49603     5  0.1364      0.960 0.012 0.000 0.000 0.036 0.952
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-4-a').click(function(){
  $('#tab-SD-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-5'>
<p><a id='tab-SD-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM49613     3  0.0881     0.9819 0.000 0.000 0.972 0.012 0.008 0.008
#&gt; GSM49604     4  0.2146     0.8904 0.000 0.116 0.000 0.880 0.000 0.004
#&gt; GSM49605     2  0.2703     0.8800 0.000 0.824 0.000 0.004 0.000 0.172
#&gt; GSM49606     2  0.2135     0.8866 0.000 0.872 0.000 0.000 0.000 0.128
#&gt; GSM49607     2  0.3215     0.8451 0.000 0.756 0.000 0.004 0.000 0.240
#&gt; GSM49608     2  0.2703     0.8800 0.000 0.824 0.000 0.004 0.000 0.172
#&gt; GSM49609     2  0.0260     0.8785 0.000 0.992 0.000 0.008 0.000 0.000
#&gt; GSM49610     2  0.0260     0.8785 0.000 0.992 0.000 0.008 0.000 0.000
#&gt; GSM49611     2  0.0260     0.8785 0.000 0.992 0.000 0.008 0.000 0.000
#&gt; GSM49612     2  0.0260     0.8785 0.000 0.992 0.000 0.008 0.000 0.000
#&gt; GSM49614     3  0.0806     0.9790 0.000 0.000 0.972 0.000 0.008 0.020
#&gt; GSM49615     3  0.0881     0.9819 0.000 0.000 0.972 0.012 0.008 0.008
#&gt; GSM49616     3  0.0146     0.9846 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; GSM49617     3  0.0520     0.9831 0.000 0.000 0.984 0.000 0.008 0.008
#&gt; GSM49564     5  0.4596     0.3718 0.024 0.000 0.000 0.016 0.612 0.348
#&gt; GSM49565     1  0.2687     0.8092 0.872 0.000 0.000 0.024 0.092 0.012
#&gt; GSM49566     5  0.4626     0.3224 0.024 0.000 0.000 0.028 0.652 0.296
#&gt; GSM49567     1  0.4872     0.7804 0.596 0.000 0.000 0.064 0.336 0.004
#&gt; GSM49568     1  0.3456     0.8415 0.788 0.000 0.000 0.040 0.172 0.000
#&gt; GSM49569     5  0.4326     0.5007 0.024 0.000 0.000 0.000 0.572 0.404
#&gt; GSM49570     4  0.3165     0.8846 0.000 0.116 0.000 0.836 0.008 0.040
#&gt; GSM49571     4  0.3456     0.7741 0.004 0.028 0.000 0.800 0.004 0.164
#&gt; GSM49572     1  0.2384     0.8247 0.884 0.000 0.000 0.032 0.084 0.000
#&gt; GSM49573     4  0.2003     0.8907 0.000 0.116 0.000 0.884 0.000 0.000
#&gt; GSM49574     1  0.1313     0.8395 0.952 0.000 0.000 0.016 0.028 0.004
#&gt; GSM49575     1  0.5104     0.8021 0.648 0.000 0.000 0.048 0.260 0.044
#&gt; GSM49576     5  0.4554     0.4796 0.024 0.000 0.000 0.008 0.568 0.400
#&gt; GSM49577     5  0.5611     0.0486 0.144 0.000 0.000 0.032 0.624 0.200
#&gt; GSM49578     1  0.3102     0.8445 0.816 0.000 0.000 0.028 0.156 0.000
#&gt; GSM49579     5  0.4707     0.3582 0.032 0.000 0.000 0.012 0.588 0.368
#&gt; GSM49580     1  0.4368     0.7846 0.656 0.000 0.000 0.048 0.296 0.000
#&gt; GSM49581     1  0.3766     0.8299 0.748 0.000 0.000 0.040 0.212 0.000
#&gt; GSM49582     1  0.3283     0.8424 0.804 0.000 0.000 0.036 0.160 0.000
#&gt; GSM49583     2  0.3519     0.8346 0.000 0.744 0.004 0.004 0.004 0.244
#&gt; GSM49584     1  0.3588     0.8403 0.776 0.000 0.000 0.044 0.180 0.000
#&gt; GSM49585     1  0.1829     0.8326 0.928 0.000 0.000 0.008 0.028 0.036
#&gt; GSM49586     6  0.4035     0.6024 0.020 0.000 0.000 0.004 0.296 0.680
#&gt; GSM49587     1  0.0653     0.8430 0.980 0.000 0.000 0.012 0.004 0.004
#&gt; GSM49588     1  0.1405     0.8357 0.948 0.000 0.000 0.004 0.024 0.024
#&gt; GSM49589     6  0.4784     0.4397 0.028 0.000 0.000 0.012 0.464 0.496
#&gt; GSM49590     5  0.4561     0.4741 0.024 0.000 0.000 0.008 0.564 0.404
#&gt; GSM49591     1  0.1173     0.8383 0.960 0.000 0.000 0.008 0.016 0.016
#&gt; GSM49592     1  0.0870     0.8404 0.972 0.000 0.000 0.012 0.004 0.012
#&gt; GSM49593     5  0.4381     0.4709 0.024 0.000 0.000 0.000 0.536 0.440
#&gt; GSM49594     6  0.4513     0.6186 0.028 0.000 0.000 0.004 0.396 0.572
#&gt; GSM49595     6  0.4513     0.6186 0.028 0.000 0.000 0.004 0.396 0.572
#&gt; GSM49596     5  0.4434     0.5004 0.028 0.000 0.000 0.000 0.544 0.428
#&gt; GSM49597     4  0.5293     0.7557 0.000 0.124 0.004 0.644 0.012 0.216
#&gt; GSM49598     5  0.4381     0.4634 0.024 0.000 0.000 0.000 0.536 0.440
#&gt; GSM49599     1  0.3943     0.8233 0.756 0.000 0.000 0.056 0.184 0.004
#&gt; GSM49600     1  0.4594     0.7285 0.600 0.000 0.000 0.032 0.360 0.008
#&gt; GSM49601     6  0.4251     0.6717 0.028 0.000 0.000 0.000 0.348 0.624
#&gt; GSM49602     6  0.4379     0.5910 0.028 0.000 0.000 0.000 0.396 0.576
#&gt; GSM49603     6  0.4379     0.5910 0.028 0.000 0.000 0.000 0.396 0.576
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-5-a').click(function(){
  $('#tab-SD-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-SD-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-get-signatures'>
<ul>
<li><a href='#tab-SD-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-kmeans-signature_compare](figure_cola/SD-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-SD-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-kmeans-collect-classes](figure_cola/SD-kmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n disease.state(p) cell.type(p) k
#> SD:kmeans 54         5.97e-07     6.72e-04 2
#> SD:kmeans 53         3.58e-07     1.61e-13 3
#> SD:kmeans 53         1.61e-06     2.73e-12 4
#> SD:kmeans 54         1.42e-07     4.23e-13 5
#> SD:kmeans 45         1.12e-05     1.37e-09 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:skmeans*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "skmeans"]
# you can also extract it by
# res = res_list["SD:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21168 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-skmeans-collect-plots](figure_cola/SD-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-skmeans-select-partition-number](figure_cola/SD-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.925           0.947       0.977         0.4506 0.547   0.547
#> 3 3 0.851           0.884       0.949         0.4884 0.704   0.494
#> 4 4 0.877           0.903       0.949         0.1116 0.879   0.655
#> 5 5 0.723           0.668       0.823         0.0624 0.977   0.912
#> 6 6 0.705           0.520       0.724         0.0457 0.977   0.907
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-get-classes'>
<ul>
<li><a href='#tab-SD-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-skmeans-get-classes-1'>
<p><a id='tab-SD-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM49613     1  0.0672      0.978 0.992 0.008
#&gt; GSM49604     2  0.0000      0.957 0.000 1.000
#&gt; GSM49605     2  0.0000      0.957 0.000 1.000
#&gt; GSM49606     2  0.0000      0.957 0.000 1.000
#&gt; GSM49607     2  0.0000      0.957 0.000 1.000
#&gt; GSM49608     2  0.0000      0.957 0.000 1.000
#&gt; GSM49609     2  0.0000      0.957 0.000 1.000
#&gt; GSM49610     2  0.0000      0.957 0.000 1.000
#&gt; GSM49611     2  0.0000      0.957 0.000 1.000
#&gt; GSM49612     2  0.0000      0.957 0.000 1.000
#&gt; GSM49614     2  0.7950      0.687 0.240 0.760
#&gt; GSM49615     1  0.0672      0.978 0.992 0.008
#&gt; GSM49616     1  0.0672      0.978 0.992 0.008
#&gt; GSM49617     1  0.4690      0.885 0.900 0.100
#&gt; GSM49564     1  0.0000      0.984 1.000 0.000
#&gt; GSM49565     1  0.0000      0.984 1.000 0.000
#&gt; GSM49566     1  0.0000      0.984 1.000 0.000
#&gt; GSM49567     1  0.0000      0.984 1.000 0.000
#&gt; GSM49568     1  0.0000      0.984 1.000 0.000
#&gt; GSM49569     1  0.0000      0.984 1.000 0.000
#&gt; GSM49570     2  0.0000      0.957 0.000 1.000
#&gt; GSM49571     2  0.0000      0.957 0.000 1.000
#&gt; GSM49572     1  0.0000      0.984 1.000 0.000
#&gt; GSM49573     2  0.0000      0.957 0.000 1.000
#&gt; GSM49574     1  0.0000      0.984 1.000 0.000
#&gt; GSM49575     1  0.5842      0.832 0.860 0.140
#&gt; GSM49576     1  0.0000      0.984 1.000 0.000
#&gt; GSM49577     2  0.9608      0.402 0.384 0.616
#&gt; GSM49578     1  0.0000      0.984 1.000 0.000
#&gt; GSM49579     1  0.0000      0.984 1.000 0.000
#&gt; GSM49580     1  0.0000      0.984 1.000 0.000
#&gt; GSM49581     1  0.0000      0.984 1.000 0.000
#&gt; GSM49582     1  0.0000      0.984 1.000 0.000
#&gt; GSM49583     2  0.0000      0.957 0.000 1.000
#&gt; GSM49584     1  0.0000      0.984 1.000 0.000
#&gt; GSM49585     1  0.0000      0.984 1.000 0.000
#&gt; GSM49586     2  0.2043      0.935 0.032 0.968
#&gt; GSM49587     1  0.0000      0.984 1.000 0.000
#&gt; GSM49588     1  0.0000      0.984 1.000 0.000
#&gt; GSM49589     1  0.0000      0.984 1.000 0.000
#&gt; GSM49590     1  0.0376      0.981 0.996 0.004
#&gt; GSM49591     1  0.0000      0.984 1.000 0.000
#&gt; GSM49592     1  0.0000      0.984 1.000 0.000
#&gt; GSM49593     1  0.0000      0.984 1.000 0.000
#&gt; GSM49594     2  0.3431      0.908 0.064 0.936
#&gt; GSM49595     1  0.7528      0.715 0.784 0.216
#&gt; GSM49596     1  0.0000      0.984 1.000 0.000
#&gt; GSM49597     2  0.0000      0.957 0.000 1.000
#&gt; GSM49598     1  0.0000      0.984 1.000 0.000
#&gt; GSM49599     1  0.2948      0.937 0.948 0.052
#&gt; GSM49600     1  0.0000      0.984 1.000 0.000
#&gt; GSM49601     1  0.0000      0.984 1.000 0.000
#&gt; GSM49602     1  0.0000      0.984 1.000 0.000
#&gt; GSM49603     1  0.0000      0.984 1.000 0.000
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-1-a').click(function(){
  $('#tab-SD-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-2'>
<p><a id='tab-SD-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM49613     3  0.0000     0.9223 0.000 0.000 1.000
#&gt; GSM49604     2  0.0000     0.9677 0.000 1.000 0.000
#&gt; GSM49605     2  0.0000     0.9677 0.000 1.000 0.000
#&gt; GSM49606     2  0.0000     0.9677 0.000 1.000 0.000
#&gt; GSM49607     2  0.0000     0.9677 0.000 1.000 0.000
#&gt; GSM49608     2  0.0000     0.9677 0.000 1.000 0.000
#&gt; GSM49609     2  0.0000     0.9677 0.000 1.000 0.000
#&gt; GSM49610     2  0.0000     0.9677 0.000 1.000 0.000
#&gt; GSM49611     2  0.0000     0.9677 0.000 1.000 0.000
#&gt; GSM49612     2  0.0000     0.9677 0.000 1.000 0.000
#&gt; GSM49614     3  0.0000     0.9223 0.000 0.000 1.000
#&gt; GSM49615     3  0.0000     0.9223 0.000 0.000 1.000
#&gt; GSM49616     3  0.0000     0.9223 0.000 0.000 1.000
#&gt; GSM49617     3  0.0000     0.9223 0.000 0.000 1.000
#&gt; GSM49564     3  0.0000     0.9223 0.000 0.000 1.000
#&gt; GSM49565     1  0.0000     0.9494 1.000 0.000 0.000
#&gt; GSM49566     3  0.2261     0.9053 0.068 0.000 0.932
#&gt; GSM49567     1  0.0000     0.9494 1.000 0.000 0.000
#&gt; GSM49568     1  0.0000     0.9494 1.000 0.000 0.000
#&gt; GSM49569     3  0.1163     0.9211 0.028 0.000 0.972
#&gt; GSM49570     2  0.0000     0.9677 0.000 1.000 0.000
#&gt; GSM49571     2  0.0000     0.9677 0.000 1.000 0.000
#&gt; GSM49572     1  0.0000     0.9494 1.000 0.000 0.000
#&gt; GSM49573     2  0.0000     0.9677 0.000 1.000 0.000
#&gt; GSM49574     1  0.0000     0.9494 1.000 0.000 0.000
#&gt; GSM49575     1  0.0237     0.9465 0.996 0.004 0.000
#&gt; GSM49576     3  0.0424     0.9226 0.008 0.000 0.992
#&gt; GSM49577     1  0.5036     0.7578 0.808 0.172 0.020
#&gt; GSM49578     1  0.0000     0.9494 1.000 0.000 0.000
#&gt; GSM49579     3  0.5216     0.6796 0.260 0.000 0.740
#&gt; GSM49580     1  0.0592     0.9408 0.988 0.000 0.012
#&gt; GSM49581     1  0.0000     0.9494 1.000 0.000 0.000
#&gt; GSM49582     1  0.0000     0.9494 1.000 0.000 0.000
#&gt; GSM49583     2  0.0000     0.9677 0.000 1.000 0.000
#&gt; GSM49584     1  0.0000     0.9494 1.000 0.000 0.000
#&gt; GSM49585     1  0.0000     0.9494 1.000 0.000 0.000
#&gt; GSM49586     2  0.7169     0.2069 0.028 0.568 0.404
#&gt; GSM49587     1  0.0000     0.9494 1.000 0.000 0.000
#&gt; GSM49588     1  0.0237     0.9468 0.996 0.000 0.004
#&gt; GSM49589     3  0.0000     0.9223 0.000 0.000 1.000
#&gt; GSM49590     3  0.0000     0.9223 0.000 0.000 1.000
#&gt; GSM49591     1  0.0000     0.9494 1.000 0.000 0.000
#&gt; GSM49592     1  0.0000     0.9494 1.000 0.000 0.000
#&gt; GSM49593     3  0.1860     0.9155 0.052 0.000 0.948
#&gt; GSM49594     3  0.7868     0.2156 0.056 0.420 0.524
#&gt; GSM49595     3  0.5435     0.8216 0.144 0.048 0.808
#&gt; GSM49596     1  0.6274     0.0846 0.544 0.000 0.456
#&gt; GSM49597     2  0.0000     0.9677 0.000 1.000 0.000
#&gt; GSM49598     3  0.3038     0.8835 0.104 0.000 0.896
#&gt; GSM49599     1  0.0237     0.9465 0.996 0.004 0.000
#&gt; GSM49600     1  0.4842     0.6864 0.776 0.000 0.224
#&gt; GSM49601     3  0.3340     0.8710 0.120 0.000 0.880
#&gt; GSM49602     3  0.1753     0.9172 0.048 0.000 0.952
#&gt; GSM49603     3  0.1753     0.9172 0.048 0.000 0.952
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-2-a').click(function(){
  $('#tab-SD-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-3'>
<p><a id='tab-SD-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM49613     3  0.0000     0.8998 0.000 0.000 1.000 0.000
#&gt; GSM49604     2  0.0000     0.9965 0.000 1.000 0.000 0.000
#&gt; GSM49605     2  0.0000     0.9965 0.000 1.000 0.000 0.000
#&gt; GSM49606     2  0.0000     0.9965 0.000 1.000 0.000 0.000
#&gt; GSM49607     2  0.0000     0.9965 0.000 1.000 0.000 0.000
#&gt; GSM49608     2  0.0000     0.9965 0.000 1.000 0.000 0.000
#&gt; GSM49609     2  0.0000     0.9965 0.000 1.000 0.000 0.000
#&gt; GSM49610     2  0.0000     0.9965 0.000 1.000 0.000 0.000
#&gt; GSM49611     2  0.0000     0.9965 0.000 1.000 0.000 0.000
#&gt; GSM49612     2  0.0000     0.9965 0.000 1.000 0.000 0.000
#&gt; GSM49614     3  0.0000     0.8998 0.000 0.000 1.000 0.000
#&gt; GSM49615     3  0.0000     0.8998 0.000 0.000 1.000 0.000
#&gt; GSM49616     3  0.0000     0.8998 0.000 0.000 1.000 0.000
#&gt; GSM49617     3  0.0000     0.8998 0.000 0.000 1.000 0.000
#&gt; GSM49564     3  0.0672     0.8921 0.008 0.000 0.984 0.008
#&gt; GSM49565     1  0.1022     0.9565 0.968 0.000 0.000 0.032
#&gt; GSM49566     4  0.5462     0.7237 0.112 0.000 0.152 0.736
#&gt; GSM49567     1  0.0376     0.9611 0.992 0.000 0.004 0.004
#&gt; GSM49568     1  0.0336     0.9619 0.992 0.000 0.000 0.008
#&gt; GSM49569     4  0.3999     0.8136 0.036 0.000 0.140 0.824
#&gt; GSM49570     2  0.0336     0.9898 0.000 0.992 0.008 0.000
#&gt; GSM49571     2  0.1118     0.9625 0.000 0.964 0.000 0.036
#&gt; GSM49572     1  0.0336     0.9608 0.992 0.000 0.000 0.008
#&gt; GSM49573     2  0.0000     0.9965 0.000 1.000 0.000 0.000
#&gt; GSM49574     1  0.0707     0.9610 0.980 0.000 0.000 0.020
#&gt; GSM49575     1  0.1004     0.9590 0.972 0.004 0.000 0.024
#&gt; GSM49576     4  0.3208     0.8179 0.004 0.000 0.148 0.848
#&gt; GSM49577     4  0.6772     0.5762 0.228 0.116 0.016 0.640
#&gt; GSM49578     1  0.0000     0.9609 1.000 0.000 0.000 0.000
#&gt; GSM49579     4  0.3088     0.8648 0.052 0.000 0.060 0.888
#&gt; GSM49580     1  0.1256     0.9516 0.964 0.000 0.028 0.008
#&gt; GSM49581     1  0.0817     0.9599 0.976 0.000 0.000 0.024
#&gt; GSM49582     1  0.0000     0.9609 1.000 0.000 0.000 0.000
#&gt; GSM49583     2  0.0000     0.9965 0.000 1.000 0.000 0.000
#&gt; GSM49584     1  0.0376     0.9613 0.992 0.000 0.004 0.004
#&gt; GSM49585     1  0.2589     0.8885 0.884 0.000 0.000 0.116
#&gt; GSM49586     4  0.0524     0.8945 0.000 0.008 0.004 0.988
#&gt; GSM49587     1  0.0000     0.9609 1.000 0.000 0.000 0.000
#&gt; GSM49588     1  0.2868     0.8613 0.864 0.000 0.000 0.136
#&gt; GSM49589     3  0.3300     0.7847 0.008 0.000 0.848 0.144
#&gt; GSM49590     3  0.4994    -0.0178 0.000 0.000 0.520 0.480
#&gt; GSM49591     1  0.1211     0.9488 0.960 0.000 0.000 0.040
#&gt; GSM49592     1  0.0336     0.9617 0.992 0.000 0.000 0.008
#&gt; GSM49593     4  0.1151     0.8931 0.008 0.000 0.024 0.968
#&gt; GSM49594     4  0.0524     0.8944 0.004 0.008 0.000 0.988
#&gt; GSM49595     4  0.0188     0.8954 0.004 0.000 0.000 0.996
#&gt; GSM49596     4  0.3658     0.7994 0.144 0.000 0.020 0.836
#&gt; GSM49597     2  0.0000     0.9965 0.000 1.000 0.000 0.000
#&gt; GSM49598     4  0.0657     0.8953 0.012 0.000 0.004 0.984
#&gt; GSM49599     1  0.2039     0.9443 0.940 0.016 0.008 0.036
#&gt; GSM49600     1  0.4236     0.8274 0.824 0.000 0.088 0.088
#&gt; GSM49601     4  0.0469     0.8953 0.012 0.000 0.000 0.988
#&gt; GSM49602     4  0.0376     0.8957 0.004 0.000 0.004 0.992
#&gt; GSM49603     4  0.0524     0.8955 0.004 0.000 0.008 0.988
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-3-a').click(function(){
  $('#tab-SD-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-4'>
<p><a id='tab-SD-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM49613     3  0.0000     0.9234 0.000 0.000 1.000 0.000 0.000
#&gt; GSM49604     2  0.3424     0.7691 0.000 0.760 0.000 0.240 0.000
#&gt; GSM49605     2  0.0000     0.8801 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49606     2  0.0000     0.8801 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49607     2  0.0000     0.8801 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49608     2  0.0162     0.8794 0.000 0.996 0.000 0.004 0.000
#&gt; GSM49609     2  0.0000     0.8801 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49610     2  0.0000     0.8801 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49611     2  0.0000     0.8801 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49612     2  0.0000     0.8801 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49614     3  0.0000     0.9234 0.000 0.000 1.000 0.000 0.000
#&gt; GSM49615     3  0.0000     0.9234 0.000 0.000 1.000 0.000 0.000
#&gt; GSM49616     3  0.0000     0.9234 0.000 0.000 1.000 0.000 0.000
#&gt; GSM49617     3  0.0000     0.9234 0.000 0.000 1.000 0.000 0.000
#&gt; GSM49564     3  0.2728     0.8450 0.004 0.000 0.888 0.040 0.068
#&gt; GSM49565     1  0.3863     0.7413 0.772 0.000 0.000 0.200 0.028
#&gt; GSM49566     5  0.6417     0.1840 0.072 0.000 0.088 0.220 0.620
#&gt; GSM49567     1  0.4138     0.6939 0.708 0.000 0.000 0.276 0.016
#&gt; GSM49568     1  0.2938     0.7754 0.876 0.000 0.008 0.084 0.032
#&gt; GSM49569     5  0.5278     0.4212 0.048 0.000 0.068 0.156 0.728
#&gt; GSM49570     2  0.4570     0.6721 0.000 0.632 0.020 0.348 0.000
#&gt; GSM49571     2  0.5148     0.5321 0.000 0.528 0.000 0.432 0.040
#&gt; GSM49572     1  0.2890     0.7741 0.836 0.000 0.000 0.160 0.004
#&gt; GSM49573     2  0.4060     0.6773 0.000 0.640 0.000 0.360 0.000
#&gt; GSM49574     1  0.2971     0.7742 0.836 0.000 0.000 0.156 0.008
#&gt; GSM49575     1  0.4682     0.5576 0.620 0.000 0.000 0.356 0.024
#&gt; GSM49576     5  0.4433     0.5162 0.004 0.000 0.076 0.156 0.764
#&gt; GSM49577     4  0.7840     0.0000 0.164 0.084 0.004 0.388 0.360
#&gt; GSM49578     1  0.0404     0.7854 0.988 0.000 0.000 0.012 0.000
#&gt; GSM49579     5  0.5526     0.3047 0.072 0.000 0.028 0.224 0.676
#&gt; GSM49580     1  0.4501     0.6747 0.740 0.000 0.012 0.212 0.036
#&gt; GSM49581     1  0.3449     0.7492 0.812 0.000 0.000 0.164 0.024
#&gt; GSM49582     1  0.1043     0.7816 0.960 0.000 0.000 0.040 0.000
#&gt; GSM49583     2  0.0510     0.8761 0.000 0.984 0.000 0.016 0.000
#&gt; GSM49584     1  0.1571     0.7831 0.936 0.000 0.004 0.060 0.000
#&gt; GSM49585     1  0.5372     0.6430 0.676 0.000 0.008 0.216 0.100
#&gt; GSM49586     5  0.4276     0.1894 0.000 0.004 0.000 0.380 0.616
#&gt; GSM49587     1  0.2179     0.7835 0.896 0.000 0.000 0.100 0.004
#&gt; GSM49588     1  0.4989     0.6530 0.708 0.000 0.000 0.168 0.124
#&gt; GSM49589     3  0.5397     0.5726 0.016 0.000 0.684 0.088 0.212
#&gt; GSM49590     5  0.4968     0.0773 0.000 0.000 0.456 0.028 0.516
#&gt; GSM49591     1  0.3639     0.7507 0.792 0.000 0.000 0.184 0.024
#&gt; GSM49592     1  0.1952     0.7799 0.912 0.000 0.000 0.084 0.004
#&gt; GSM49593     5  0.1851     0.5926 0.000 0.000 0.000 0.088 0.912
#&gt; GSM49594     5  0.4497     0.4784 0.008 0.044 0.004 0.188 0.756
#&gt; GSM49595     5  0.2690     0.5690 0.000 0.000 0.000 0.156 0.844
#&gt; GSM49596     5  0.4855     0.3220 0.112 0.000 0.000 0.168 0.720
#&gt; GSM49597     2  0.2690     0.8190 0.000 0.844 0.000 0.156 0.000
#&gt; GSM49598     5  0.2237     0.5976 0.008 0.000 0.004 0.084 0.904
#&gt; GSM49599     1  0.4630     0.6913 0.672 0.000 0.008 0.300 0.020
#&gt; GSM49600     1  0.7153     0.1950 0.524 0.000 0.056 0.240 0.180
#&gt; GSM49601     5  0.2338     0.5884 0.004 0.000 0.000 0.112 0.884
#&gt; GSM49602     5  0.0963     0.6163 0.000 0.000 0.000 0.036 0.964
#&gt; GSM49603     5  0.0880     0.6165 0.000 0.000 0.000 0.032 0.968
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-4-a').click(function(){
  $('#tab-SD-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-5'>
<p><a id='tab-SD-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM49613     3  0.0000     0.8831 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49604     2  0.3727    -0.4777 0.000 0.612 0.000 0.388 0.000 0.000
#&gt; GSM49605     2  0.0000     0.8557 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49606     2  0.0291     0.8565 0.000 0.992 0.000 0.004 0.000 0.004
#&gt; GSM49607     2  0.0260     0.8524 0.000 0.992 0.000 0.008 0.000 0.000
#&gt; GSM49608     2  0.0260     0.8524 0.000 0.992 0.000 0.008 0.000 0.000
#&gt; GSM49609     2  0.0508     0.8535 0.000 0.984 0.000 0.012 0.000 0.004
#&gt; GSM49610     2  0.0291     0.8565 0.000 0.992 0.000 0.004 0.000 0.004
#&gt; GSM49611     2  0.0508     0.8535 0.000 0.984 0.000 0.012 0.000 0.004
#&gt; GSM49612     2  0.0508     0.8535 0.000 0.984 0.000 0.012 0.000 0.004
#&gt; GSM49614     3  0.0000     0.8831 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49615     3  0.0000     0.8831 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49616     3  0.0000     0.8831 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49617     3  0.0000     0.8831 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49564     3  0.4199     0.7071 0.004 0.000 0.780 0.036 0.128 0.052
#&gt; GSM49565     1  0.4538     0.4815 0.536 0.000 0.000 0.020 0.008 0.436
#&gt; GSM49566     5  0.7506    -0.0271 0.080 0.000 0.060 0.148 0.480 0.232
#&gt; GSM49567     1  0.5001     0.4402 0.596 0.000 0.000 0.096 0.000 0.308
#&gt; GSM49568     1  0.3141     0.6133 0.852 0.000 0.000 0.040 0.024 0.084
#&gt; GSM49569     5  0.6579     0.2721 0.036 0.000 0.092 0.112 0.604 0.156
#&gt; GSM49570     4  0.3989     0.8373 0.000 0.468 0.004 0.528 0.000 0.000
#&gt; GSM49571     4  0.4479     0.7837 0.000 0.356 0.000 0.608 0.032 0.004
#&gt; GSM49572     1  0.4416     0.5361 0.600 0.000 0.000 0.020 0.008 0.372
#&gt; GSM49573     4  0.3838     0.8615 0.000 0.448 0.000 0.552 0.000 0.000
#&gt; GSM49574     1  0.3897     0.6043 0.696 0.000 0.000 0.024 0.000 0.280
#&gt; GSM49575     1  0.6066     0.3613 0.500 0.000 0.000 0.260 0.012 0.228
#&gt; GSM49576     5  0.5882     0.3439 0.004 0.000 0.100 0.104 0.644 0.148
#&gt; GSM49577     6  0.7626     0.0000 0.092 0.040 0.004 0.172 0.228 0.464
#&gt; GSM49578     1  0.1398     0.6345 0.940 0.000 0.000 0.008 0.000 0.052
#&gt; GSM49579     5  0.6263    -0.0650 0.028 0.000 0.012 0.116 0.484 0.360
#&gt; GSM49580     1  0.6369     0.3784 0.592 0.000 0.020 0.092 0.080 0.216
#&gt; GSM49581     1  0.5211     0.5016 0.680 0.000 0.000 0.076 0.056 0.188
#&gt; GSM49582     1  0.1196     0.6297 0.952 0.000 0.000 0.008 0.000 0.040
#&gt; GSM49583     2  0.1007     0.8113 0.000 0.956 0.000 0.044 0.000 0.000
#&gt; GSM49584     1  0.2701     0.6088 0.864 0.000 0.004 0.028 0.000 0.104
#&gt; GSM49585     1  0.6137     0.4545 0.492 0.000 0.008 0.056 0.068 0.376
#&gt; GSM49586     5  0.4720     0.2046 0.000 0.000 0.000 0.388 0.560 0.052
#&gt; GSM49587     1  0.3349     0.6091 0.748 0.000 0.000 0.008 0.000 0.244
#&gt; GSM49588     1  0.5749     0.4935 0.532 0.000 0.000 0.044 0.072 0.352
#&gt; GSM49589     3  0.6328     0.3875 0.008 0.000 0.556 0.080 0.268 0.088
#&gt; GSM49590     5  0.6149     0.1184 0.000 0.000 0.368 0.088 0.484 0.060
#&gt; GSM49591     1  0.4385     0.5786 0.636 0.000 0.000 0.032 0.004 0.328
#&gt; GSM49592     1  0.3384     0.6077 0.760 0.000 0.000 0.008 0.004 0.228
#&gt; GSM49593     5  0.4011     0.4529 0.008 0.000 0.016 0.080 0.796 0.100
#&gt; GSM49594     5  0.6091     0.2650 0.008 0.036 0.000 0.164 0.588 0.204
#&gt; GSM49595     5  0.4923     0.3515 0.004 0.000 0.000 0.144 0.668 0.184
#&gt; GSM49596     5  0.6703     0.1347 0.136 0.000 0.012 0.108 0.560 0.184
#&gt; GSM49597     2  0.3555     0.0886 0.000 0.712 0.000 0.280 0.000 0.008
#&gt; GSM49598     5  0.4319     0.4353 0.020 0.000 0.004 0.104 0.768 0.104
#&gt; GSM49599     1  0.6250     0.3527 0.432 0.004 0.000 0.188 0.012 0.364
#&gt; GSM49600     1  0.8156    -0.0327 0.384 0.000 0.072 0.116 0.180 0.248
#&gt; GSM49601     5  0.3566     0.4659 0.016 0.000 0.000 0.080 0.820 0.084
#&gt; GSM49602     5  0.1434     0.5063 0.000 0.000 0.000 0.048 0.940 0.012
#&gt; GSM49603     5  0.1549     0.5066 0.000 0.000 0.000 0.044 0.936 0.020
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-5-a').click(function(){
  $('#tab-SD-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-SD-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-get-signatures'>
<ul>
<li><a href='#tab-SD-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-skmeans-signature_compare](figure_cola/SD-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-SD-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-skmeans-collect-classes](figure_cola/SD-skmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n disease.state(p) cell.type(p) k
#> SD:skmeans 53         6.87e-05     9.70e-03 2
#> SD:skmeans 51         4.51e-06     3.34e-04 3
#> SD:skmeans 53         7.85e-06     1.27e-08 4
#> SD:skmeans 45         8.51e-05     5.35e-07 5
#> SD:skmeans 30         2.20e-04     4.12e-05 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:pam






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "pam"]
# you can also extract it by
# res = res_list["SD:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21168 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-pam-collect-plots](figure_cola/SD-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-pam-select-partition-number](figure_cola/SD-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.581           0.936       0.910         0.3156 0.669   0.669
#> 3 3 0.806           0.916       0.943         0.5316 0.867   0.802
#> 4 4 0.867           0.907       0.955         0.4619 0.751   0.535
#> 5 5 0.845           0.894       0.957         0.0564 0.965   0.879
#> 6 6 0.811           0.822       0.912         0.0294 0.974   0.900
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-pam-get-classes'>
<ul>
<li><a href='#tab-SD-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-pam-get-classes-1'>
<p><a id='tab-SD-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM49613     1  0.5408      0.848 0.876 0.124
#&gt; GSM49604     2  0.5408      0.970 0.124 0.876
#&gt; GSM49605     2  0.5408      0.970 0.124 0.876
#&gt; GSM49606     2  0.5408      0.970 0.124 0.876
#&gt; GSM49607     2  0.5408      0.970 0.124 0.876
#&gt; GSM49608     2  0.5408      0.970 0.124 0.876
#&gt; GSM49609     2  0.5408      0.970 0.124 0.876
#&gt; GSM49610     2  0.5408      0.970 0.124 0.876
#&gt; GSM49611     2  0.5408      0.970 0.124 0.876
#&gt; GSM49612     2  0.5408      0.970 0.124 0.876
#&gt; GSM49614     1  0.5946      0.850 0.856 0.144
#&gt; GSM49615     1  0.5408      0.848 0.876 0.124
#&gt; GSM49616     1  0.5842      0.850 0.860 0.140
#&gt; GSM49617     1  0.5946      0.850 0.856 0.144
#&gt; GSM49564     1  0.0000      0.964 1.000 0.000
#&gt; GSM49565     1  0.0000      0.964 1.000 0.000
#&gt; GSM49566     1  0.0000      0.964 1.000 0.000
#&gt; GSM49567     1  0.0000      0.964 1.000 0.000
#&gt; GSM49568     1  0.0000      0.964 1.000 0.000
#&gt; GSM49569     1  0.0938      0.962 0.988 0.012
#&gt; GSM49570     1  0.9129      0.406 0.672 0.328
#&gt; GSM49571     1  0.1414      0.961 0.980 0.020
#&gt; GSM49572     1  0.0000      0.964 1.000 0.000
#&gt; GSM49573     1  0.1414      0.961 0.980 0.020
#&gt; GSM49574     1  0.0000      0.964 1.000 0.000
#&gt; GSM49575     1  0.0000      0.964 1.000 0.000
#&gt; GSM49576     1  0.1414      0.961 0.980 0.020
#&gt; GSM49577     1  0.1414      0.961 0.980 0.020
#&gt; GSM49578     1  0.0000      0.964 1.000 0.000
#&gt; GSM49579     1  0.1414      0.961 0.980 0.020
#&gt; GSM49580     1  0.0000      0.964 1.000 0.000
#&gt; GSM49581     1  0.0000      0.964 1.000 0.000
#&gt; GSM49582     1  0.0000      0.964 1.000 0.000
#&gt; GSM49583     2  0.5408      0.970 0.124 0.876
#&gt; GSM49584     1  0.0000      0.964 1.000 0.000
#&gt; GSM49585     1  0.0000      0.964 1.000 0.000
#&gt; GSM49586     1  0.1414      0.961 0.980 0.020
#&gt; GSM49587     1  0.0000      0.964 1.000 0.000
#&gt; GSM49588     1  0.0000      0.964 1.000 0.000
#&gt; GSM49589     1  0.1414      0.961 0.980 0.020
#&gt; GSM49590     1  0.1414      0.961 0.980 0.020
#&gt; GSM49591     1  0.0000      0.964 1.000 0.000
#&gt; GSM49592     1  0.0000      0.964 1.000 0.000
#&gt; GSM49593     1  0.1414      0.961 0.980 0.020
#&gt; GSM49594     1  0.1414      0.961 0.980 0.020
#&gt; GSM49595     1  0.1414      0.961 0.980 0.020
#&gt; GSM49596     1  0.0000      0.964 1.000 0.000
#&gt; GSM49597     2  0.9552      0.573 0.376 0.624
#&gt; GSM49598     1  0.1414      0.961 0.980 0.020
#&gt; GSM49599     1  0.0376      0.964 0.996 0.004
#&gt; GSM49600     1  0.0000      0.964 1.000 0.000
#&gt; GSM49601     1  0.1414      0.961 0.980 0.020
#&gt; GSM49602     1  0.1414      0.961 0.980 0.020
#&gt; GSM49603     1  0.1414      0.961 0.980 0.020
</code></pre>

<script>
$('#tab-SD-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-1-a').click(function(){
  $('#tab-SD-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-2'>
<p><a id='tab-SD-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM49613     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM49604     2  0.0000      0.942 0.000 1.000 0.000
#&gt; GSM49605     2  0.0000      0.942 0.000 1.000 0.000
#&gt; GSM49606     2  0.0000      0.942 0.000 1.000 0.000
#&gt; GSM49607     2  0.0000      0.942 0.000 1.000 0.000
#&gt; GSM49608     2  0.0000      0.942 0.000 1.000 0.000
#&gt; GSM49609     2  0.0000      0.942 0.000 1.000 0.000
#&gt; GSM49610     2  0.0000      0.942 0.000 1.000 0.000
#&gt; GSM49611     2  0.0000      0.942 0.000 1.000 0.000
#&gt; GSM49612     2  0.0000      0.942 0.000 1.000 0.000
#&gt; GSM49614     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM49615     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM49616     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM49617     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM49564     1  0.2711      0.928 0.912 0.000 0.088
#&gt; GSM49565     1  0.0237      0.925 0.996 0.004 0.000
#&gt; GSM49566     1  0.2945      0.928 0.908 0.004 0.088
#&gt; GSM49567     1  0.0000      0.926 1.000 0.000 0.000
#&gt; GSM49568     1  0.0000      0.926 1.000 0.000 0.000
#&gt; GSM49569     1  0.3587      0.927 0.892 0.020 0.088
#&gt; GSM49570     1  0.8470      0.422 0.552 0.344 0.104
#&gt; GSM49571     1  0.4092      0.925 0.876 0.036 0.088
#&gt; GSM49572     1  0.0000      0.926 1.000 0.000 0.000
#&gt; GSM49573     1  0.3973      0.926 0.880 0.032 0.088
#&gt; GSM49574     1  0.0000      0.926 1.000 0.000 0.000
#&gt; GSM49575     1  0.0000      0.926 1.000 0.000 0.000
#&gt; GSM49576     1  0.4092      0.925 0.876 0.036 0.088
#&gt; GSM49577     1  0.2806      0.929 0.928 0.032 0.040
#&gt; GSM49578     1  0.0000      0.926 1.000 0.000 0.000
#&gt; GSM49579     1  0.4092      0.925 0.876 0.036 0.088
#&gt; GSM49580     1  0.0000      0.926 1.000 0.000 0.000
#&gt; GSM49581     1  0.0000      0.926 1.000 0.000 0.000
#&gt; GSM49582     1  0.0000      0.926 1.000 0.000 0.000
#&gt; GSM49583     2  0.0000      0.942 0.000 1.000 0.000
#&gt; GSM49584     1  0.0000      0.926 1.000 0.000 0.000
#&gt; GSM49585     1  0.0237      0.926 0.996 0.000 0.004
#&gt; GSM49586     1  0.4092      0.925 0.876 0.036 0.088
#&gt; GSM49587     1  0.0000      0.926 1.000 0.000 0.000
#&gt; GSM49588     1  0.2625      0.928 0.916 0.000 0.084
#&gt; GSM49589     1  0.4092      0.925 0.876 0.036 0.088
#&gt; GSM49590     1  0.4092      0.925 0.876 0.036 0.088
#&gt; GSM49591     1  0.0000      0.926 1.000 0.000 0.000
#&gt; GSM49592     1  0.0000      0.926 1.000 0.000 0.000
#&gt; GSM49593     1  0.3973      0.926 0.880 0.032 0.088
#&gt; GSM49594     1  0.4092      0.925 0.876 0.036 0.088
#&gt; GSM49595     1  0.4092      0.925 0.876 0.036 0.088
#&gt; GSM49596     1  0.3129      0.928 0.904 0.008 0.088
#&gt; GSM49597     2  0.6255      0.385 0.320 0.668 0.012
#&gt; GSM49598     1  0.3973      0.926 0.880 0.032 0.088
#&gt; GSM49599     1  0.0237      0.926 0.996 0.004 0.000
#&gt; GSM49600     1  0.0237      0.926 0.996 0.000 0.004
#&gt; GSM49601     1  0.4092      0.925 0.876 0.036 0.088
#&gt; GSM49602     1  0.4092      0.925 0.876 0.036 0.088
#&gt; GSM49603     1  0.4092      0.925 0.876 0.036 0.088
</code></pre>

<script>
$('#tab-SD-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-2-a').click(function(){
  $('#tab-SD-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-3'>
<p><a id='tab-SD-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2 p3    p4
#&gt; GSM49613     3  0.0000      1.000 0.000 0.000  1 0.000
#&gt; GSM49604     2  0.0000      0.947 0.000 1.000  0 0.000
#&gt; GSM49605     2  0.0000      0.947 0.000 1.000  0 0.000
#&gt; GSM49606     2  0.0000      0.947 0.000 1.000  0 0.000
#&gt; GSM49607     2  0.0000      0.947 0.000 1.000  0 0.000
#&gt; GSM49608     2  0.0000      0.947 0.000 1.000  0 0.000
#&gt; GSM49609     2  0.0000      0.947 0.000 1.000  0 0.000
#&gt; GSM49610     2  0.0000      0.947 0.000 1.000  0 0.000
#&gt; GSM49611     2  0.0000      0.947 0.000 1.000  0 0.000
#&gt; GSM49612     2  0.0000      0.947 0.000 1.000  0 0.000
#&gt; GSM49614     3  0.0000      1.000 0.000 0.000  1 0.000
#&gt; GSM49615     3  0.0000      1.000 0.000 0.000  1 0.000
#&gt; GSM49616     3  0.0000      1.000 0.000 0.000  1 0.000
#&gt; GSM49617     3  0.0000      1.000 0.000 0.000  1 0.000
#&gt; GSM49564     4  0.0000      0.937 0.000 0.000  0 1.000
#&gt; GSM49565     1  0.1474      0.932 0.948 0.000  0 0.052
#&gt; GSM49566     4  0.0336      0.935 0.008 0.000  0 0.992
#&gt; GSM49567     1  0.0592      0.947 0.984 0.000  0 0.016
#&gt; GSM49568     1  0.0921      0.945 0.972 0.000  0 0.028
#&gt; GSM49569     4  0.0000      0.937 0.000 0.000  0 1.000
#&gt; GSM49570     4  0.2142      0.902 0.056 0.016  0 0.928
#&gt; GSM49571     4  0.2011      0.892 0.080 0.000  0 0.920
#&gt; GSM49572     1  0.0592      0.947 0.984 0.000  0 0.016
#&gt; GSM49573     4  0.2921      0.856 0.140 0.000  0 0.860
#&gt; GSM49574     1  0.0817      0.945 0.976 0.000  0 0.024
#&gt; GSM49575     1  0.1940      0.907 0.924 0.000  0 0.076
#&gt; GSM49576     4  0.2281      0.877 0.096 0.000  0 0.904
#&gt; GSM49577     4  0.4888      0.272 0.412 0.000  0 0.588
#&gt; GSM49578     1  0.0592      0.947 0.984 0.000  0 0.016
#&gt; GSM49579     4  0.2530      0.863 0.112 0.000  0 0.888
#&gt; GSM49580     1  0.0921      0.945 0.972 0.000  0 0.028
#&gt; GSM49581     1  0.0592      0.947 0.984 0.000  0 0.016
#&gt; GSM49582     1  0.0592      0.947 0.984 0.000  0 0.016
#&gt; GSM49583     2  0.0000      0.947 0.000 1.000  0 0.000
#&gt; GSM49584     1  0.0592      0.947 0.984 0.000  0 0.016
#&gt; GSM49585     1  0.2530      0.883 0.888 0.000  0 0.112
#&gt; GSM49586     4  0.0000      0.937 0.000 0.000  0 1.000
#&gt; GSM49587     1  0.0817      0.946 0.976 0.000  0 0.024
#&gt; GSM49588     4  0.3311      0.800 0.172 0.000  0 0.828
#&gt; GSM49589     4  0.0000      0.937 0.000 0.000  0 1.000
#&gt; GSM49590     4  0.0000      0.937 0.000 0.000  0 1.000
#&gt; GSM49591     1  0.1302      0.939 0.956 0.000  0 0.044
#&gt; GSM49592     1  0.1118      0.943 0.964 0.000  0 0.036
#&gt; GSM49593     4  0.0000      0.937 0.000 0.000  0 1.000
#&gt; GSM49594     4  0.0000      0.937 0.000 0.000  0 1.000
#&gt; GSM49595     4  0.0000      0.937 0.000 0.000  0 1.000
#&gt; GSM49596     4  0.0336      0.935 0.008 0.000  0 0.992
#&gt; GSM49597     2  0.5298      0.370 0.016 0.612  0 0.372
#&gt; GSM49598     4  0.0469      0.932 0.012 0.000  0 0.988
#&gt; GSM49599     1  0.3486      0.786 0.812 0.000  0 0.188
#&gt; GSM49600     1  0.3975      0.721 0.760 0.000  0 0.240
#&gt; GSM49601     4  0.0000      0.937 0.000 0.000  0 1.000
#&gt; GSM49602     4  0.0000      0.937 0.000 0.000  0 1.000
#&gt; GSM49603     4  0.0000      0.937 0.000 0.000  0 1.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-3-a').click(function(){
  $('#tab-SD-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-4'>
<p><a id='tab-SD-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2 p3    p4    p5
#&gt; GSM49613     3  0.0000      1.000 0.000 0.000  1 0.000 0.000
#&gt; GSM49604     2  0.3913      0.530 0.000 0.676  0 0.324 0.000
#&gt; GSM49605     2  0.0000      0.962 0.000 1.000  0 0.000 0.000
#&gt; GSM49606     2  0.0000      0.962 0.000 1.000  0 0.000 0.000
#&gt; GSM49607     2  0.0000      0.962 0.000 1.000  0 0.000 0.000
#&gt; GSM49608     2  0.0162      0.959 0.000 0.996  0 0.004 0.000
#&gt; GSM49609     2  0.0000      0.962 0.000 1.000  0 0.000 0.000
#&gt; GSM49610     2  0.0000      0.962 0.000 1.000  0 0.000 0.000
#&gt; GSM49611     2  0.0000      0.962 0.000 1.000  0 0.000 0.000
#&gt; GSM49612     2  0.0000      0.962 0.000 1.000  0 0.000 0.000
#&gt; GSM49614     3  0.0000      1.000 0.000 0.000  1 0.000 0.000
#&gt; GSM49615     3  0.0000      1.000 0.000 0.000  1 0.000 0.000
#&gt; GSM49616     3  0.0000      1.000 0.000 0.000  1 0.000 0.000
#&gt; GSM49617     3  0.0000      1.000 0.000 0.000  1 0.000 0.000
#&gt; GSM49564     5  0.0000      0.919 0.000 0.000  0 0.000 1.000
#&gt; GSM49565     1  0.0000      0.948 1.000 0.000  0 0.000 0.000
#&gt; GSM49566     5  0.0703      0.905 0.024 0.000  0 0.000 0.976
#&gt; GSM49567     1  0.0000      0.948 1.000 0.000  0 0.000 0.000
#&gt; GSM49568     1  0.0000      0.948 1.000 0.000  0 0.000 0.000
#&gt; GSM49569     5  0.0000      0.919 0.000 0.000  0 0.000 1.000
#&gt; GSM49570     4  0.0000      0.852 0.000 0.000  0 1.000 0.000
#&gt; GSM49571     5  0.3659      0.679 0.012 0.000  0 0.220 0.768
#&gt; GSM49572     1  0.0000      0.948 1.000 0.000  0 0.000 0.000
#&gt; GSM49573     4  0.0000      0.852 0.000 0.000  0 1.000 0.000
#&gt; GSM49574     1  0.0162      0.947 0.996 0.000  0 0.000 0.004
#&gt; GSM49575     1  0.1341      0.907 0.944 0.000  0 0.000 0.056
#&gt; GSM49576     5  0.2074      0.836 0.104 0.000  0 0.000 0.896
#&gt; GSM49577     5  0.4227      0.256 0.420 0.000  0 0.000 0.580
#&gt; GSM49578     1  0.0000      0.948 1.000 0.000  0 0.000 0.000
#&gt; GSM49579     5  0.2230      0.823 0.116 0.000  0 0.000 0.884
#&gt; GSM49580     1  0.0404      0.943 0.988 0.000  0 0.000 0.012
#&gt; GSM49581     1  0.0000      0.948 1.000 0.000  0 0.000 0.000
#&gt; GSM49582     1  0.0000      0.948 1.000 0.000  0 0.000 0.000
#&gt; GSM49583     2  0.0000      0.962 0.000 1.000  0 0.000 0.000
#&gt; GSM49584     1  0.0000      0.948 1.000 0.000  0 0.000 0.000
#&gt; GSM49585     1  0.2329      0.842 0.876 0.000  0 0.000 0.124
#&gt; GSM49586     5  0.1341      0.881 0.000 0.000  0 0.056 0.944
#&gt; GSM49587     1  0.0000      0.948 1.000 0.000  0 0.000 0.000
#&gt; GSM49588     5  0.2852      0.755 0.172 0.000  0 0.000 0.828
#&gt; GSM49589     5  0.0000      0.919 0.000 0.000  0 0.000 1.000
#&gt; GSM49590     5  0.0000      0.919 0.000 0.000  0 0.000 1.000
#&gt; GSM49591     1  0.1544      0.900 0.932 0.000  0 0.000 0.068
#&gt; GSM49592     1  0.0609      0.938 0.980 0.000  0 0.000 0.020
#&gt; GSM49593     5  0.0000      0.919 0.000 0.000  0 0.000 1.000
#&gt; GSM49594     5  0.0000      0.919 0.000 0.000  0 0.000 1.000
#&gt; GSM49595     5  0.0000      0.919 0.000 0.000  0 0.000 1.000
#&gt; GSM49596     5  0.0290      0.915 0.008 0.000  0 0.000 0.992
#&gt; GSM49597     4  0.4075      0.703 0.000 0.060  0 0.780 0.160
#&gt; GSM49598     5  0.0000      0.919 0.000 0.000  0 0.000 1.000
#&gt; GSM49599     1  0.2516      0.821 0.860 0.000  0 0.000 0.140
#&gt; GSM49600     1  0.3210      0.721 0.788 0.000  0 0.000 0.212
#&gt; GSM49601     5  0.0000      0.919 0.000 0.000  0 0.000 1.000
#&gt; GSM49602     5  0.0000      0.919 0.000 0.000  0 0.000 1.000
#&gt; GSM49603     5  0.0000      0.919 0.000 0.000  0 0.000 1.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-4-a').click(function(){
  $('#tab-SD-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-5'>
<p><a id='tab-SD-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2 p3    p4    p5    p6
#&gt; GSM49613     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 0.000
#&gt; GSM49604     4  0.3998      0.429 0.000 0.340  0 0.644 0.000 0.016
#&gt; GSM49605     6  0.2697      0.739 0.000 0.188  0 0.000 0.000 0.812
#&gt; GSM49606     2  0.3851     -0.285 0.000 0.540  0 0.000 0.000 0.460
#&gt; GSM49607     6  0.2631      0.739 0.000 0.180  0 0.000 0.000 0.820
#&gt; GSM49608     6  0.2762      0.734 0.000 0.196  0 0.000 0.000 0.804
#&gt; GSM49609     2  0.0000      0.836 0.000 1.000  0 0.000 0.000 0.000
#&gt; GSM49610     2  0.0000      0.836 0.000 1.000  0 0.000 0.000 0.000
#&gt; GSM49611     2  0.0000      0.836 0.000 1.000  0 0.000 0.000 0.000
#&gt; GSM49612     2  0.0000      0.836 0.000 1.000  0 0.000 0.000 0.000
#&gt; GSM49614     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 0.000
#&gt; GSM49615     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 0.000
#&gt; GSM49616     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 0.000
#&gt; GSM49617     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 0.000
#&gt; GSM49564     5  0.0000      0.909 0.000 0.000  0 0.000 1.000 0.000
#&gt; GSM49565     1  0.1007      0.932 0.956 0.000  0 0.000 0.000 0.044
#&gt; GSM49566     5  0.0632      0.899 0.024 0.000  0 0.000 0.976 0.000
#&gt; GSM49567     1  0.0713      0.936 0.972 0.000  0 0.000 0.000 0.028
#&gt; GSM49568     1  0.0547      0.937 0.980 0.000  0 0.000 0.000 0.020
#&gt; GSM49569     5  0.0000      0.909 0.000 0.000  0 0.000 1.000 0.000
#&gt; GSM49570     4  0.0146      0.780 0.000 0.000  0 0.996 0.000 0.004
#&gt; GSM49571     5  0.3780      0.719 0.016 0.000  0 0.204 0.760 0.020
#&gt; GSM49572     1  0.0547      0.938 0.980 0.000  0 0.000 0.000 0.020
#&gt; GSM49573     4  0.0000      0.781 0.000 0.000  0 1.000 0.000 0.000
#&gt; GSM49574     1  0.0632      0.937 0.976 0.000  0 0.000 0.000 0.024
#&gt; GSM49575     1  0.1765      0.903 0.924 0.000  0 0.000 0.052 0.024
#&gt; GSM49576     5  0.2537      0.832 0.096 0.000  0 0.000 0.872 0.032
#&gt; GSM49577     5  0.5303      0.385 0.332 0.000  0 0.000 0.548 0.120
#&gt; GSM49578     1  0.0547      0.937 0.980 0.000  0 0.000 0.000 0.020
#&gt; GSM49579     5  0.3041      0.788 0.128 0.000  0 0.000 0.832 0.040
#&gt; GSM49580     1  0.1124      0.933 0.956 0.000  0 0.000 0.008 0.036
#&gt; GSM49581     1  0.0632      0.936 0.976 0.000  0 0.000 0.000 0.024
#&gt; GSM49582     1  0.0547      0.937 0.980 0.000  0 0.000 0.000 0.020
#&gt; GSM49583     6  0.3854      0.175 0.000 0.464  0 0.000 0.000 0.536
#&gt; GSM49584     1  0.0458      0.937 0.984 0.000  0 0.000 0.000 0.016
#&gt; GSM49585     1  0.2999      0.830 0.836 0.000  0 0.000 0.124 0.040
#&gt; GSM49586     5  0.1151      0.894 0.000 0.000  0 0.032 0.956 0.012
#&gt; GSM49587     1  0.0865      0.933 0.964 0.000  0 0.000 0.000 0.036
#&gt; GSM49588     5  0.3455      0.726 0.180 0.000  0 0.000 0.784 0.036
#&gt; GSM49589     5  0.0000      0.909 0.000 0.000  0 0.000 1.000 0.000
#&gt; GSM49590     5  0.0000      0.909 0.000 0.000  0 0.000 1.000 0.000
#&gt; GSM49591     1  0.2294      0.886 0.892 0.000  0 0.000 0.072 0.036
#&gt; GSM49592     1  0.1564      0.929 0.936 0.000  0 0.000 0.024 0.040
#&gt; GSM49593     5  0.0000      0.909 0.000 0.000  0 0.000 1.000 0.000
#&gt; GSM49594     5  0.1910      0.856 0.000 0.000  0 0.000 0.892 0.108
#&gt; GSM49595     5  0.1910      0.856 0.000 0.000  0 0.000 0.892 0.108
#&gt; GSM49596     5  0.0260      0.907 0.008 0.000  0 0.000 0.992 0.000
#&gt; GSM49597     6  0.4151      0.387 0.000 0.000  0 0.264 0.044 0.692
#&gt; GSM49598     5  0.0000      0.909 0.000 0.000  0 0.000 1.000 0.000
#&gt; GSM49599     1  0.2199      0.878 0.892 0.000  0 0.000 0.088 0.020
#&gt; GSM49600     1  0.2981      0.793 0.820 0.000  0 0.000 0.160 0.020
#&gt; GSM49601     5  0.0146      0.908 0.000 0.000  0 0.000 0.996 0.004
#&gt; GSM49602     5  0.0000      0.909 0.000 0.000  0 0.000 1.000 0.000
#&gt; GSM49603     5  0.0000      0.909 0.000 0.000  0 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-5-a').click(function(){
  $('#tab-SD-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-pam-membership-heatmap'>
<ul>
<li><a href='#tab-SD-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-pam-get-signatures'>
<ul>
<li><a href='#tab-SD-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-1-1.png" alt="plot of chunk tab-SD-pam-get-signatures-1"/></p>

</div>
<div id='tab-SD-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-2-1.png" alt="plot of chunk tab-SD-pam-get-signatures-2"/></p>

</div>
<div id='tab-SD-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-3-1.png" alt="plot of chunk tab-SD-pam-get-signatures-3"/></p>

</div>
<div id='tab-SD-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-4-1.png" alt="plot of chunk tab-SD-pam-get-signatures-4"/></p>

</div>
<div id='tab-SD-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-5-1.png" alt="plot of chunk tab-SD-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-pam-signature_compare](figure_cola/SD-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-pam-dimension-reduction'>
<ul>
<li><a href='#tab-SD-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-pam-collect-classes](figure_cola/SD-pam-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>         n disease.state(p) cell.type(p) k
#> SD:pam 53         2.67e-08     1.83e-04 2
#> SD:pam 52         1.22e-09     2.39e-14 3
#> SD:pam 52         6.37e-09     4.29e-13 4
#> SD:pam 53         1.75e-08     2.70e-12 5
#> SD:pam 49         3.46e-08     1.37e-16 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:mclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "mclust"]
# you can also extract it by
# res = res_list["SD:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21168 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-mclust-collect-plots](figure_cola/SD-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-mclust-select-partition-number](figure_cola/SD-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.978       0.990         0.4700 0.535   0.535
#> 3 3 0.978           0.929       0.963         0.1056 0.937   0.883
#> 4 4 0.862           0.925       0.935         0.1002 0.983   0.963
#> 5 5 0.642           0.692       0.823         0.2399 0.777   0.521
#> 6 6 0.615           0.553       0.749         0.0953 0.818   0.437
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-get-classes'>
<ul>
<li><a href='#tab-SD-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-mclust-get-classes-1'>
<p><a id='tab-SD-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM49613     2  0.0376      0.996 0.004 0.996
#&gt; GSM49604     2  0.0000      0.998 0.000 1.000
#&gt; GSM49605     2  0.0000      0.998 0.000 1.000
#&gt; GSM49606     2  0.0000      0.998 0.000 1.000
#&gt; GSM49607     2  0.0000      0.998 0.000 1.000
#&gt; GSM49608     2  0.0000      0.998 0.000 1.000
#&gt; GSM49609     2  0.0000      0.998 0.000 1.000
#&gt; GSM49610     2  0.0000      0.998 0.000 1.000
#&gt; GSM49611     2  0.0000      0.998 0.000 1.000
#&gt; GSM49612     2  0.0000      0.998 0.000 1.000
#&gt; GSM49614     2  0.0376      0.996 0.004 0.996
#&gt; GSM49615     2  0.0376      0.996 0.004 0.996
#&gt; GSM49616     2  0.0376      0.996 0.004 0.996
#&gt; GSM49617     2  0.0376      0.996 0.004 0.996
#&gt; GSM49564     1  0.0376      0.981 0.996 0.004
#&gt; GSM49565     1  0.0000      0.985 1.000 0.000
#&gt; GSM49566     1  0.0000      0.985 1.000 0.000
#&gt; GSM49567     1  0.0000      0.985 1.000 0.000
#&gt; GSM49568     1  0.0000      0.985 1.000 0.000
#&gt; GSM49569     1  0.0000      0.985 1.000 0.000
#&gt; GSM49570     2  0.0000      0.998 0.000 1.000
#&gt; GSM49571     2  0.0938      0.988 0.012 0.988
#&gt; GSM49572     1  0.0000      0.985 1.000 0.000
#&gt; GSM49573     2  0.0000      0.998 0.000 1.000
#&gt; GSM49574     1  0.0000      0.985 1.000 0.000
#&gt; GSM49575     1  0.8081      0.677 0.752 0.248
#&gt; GSM49576     1  0.0000      0.985 1.000 0.000
#&gt; GSM49577     1  0.0000      0.985 1.000 0.000
#&gt; GSM49578     1  0.0000      0.985 1.000 0.000
#&gt; GSM49579     1  0.0000      0.985 1.000 0.000
#&gt; GSM49580     1  0.0000      0.985 1.000 0.000
#&gt; GSM49581     1  0.0000      0.985 1.000 0.000
#&gt; GSM49582     1  0.0000      0.985 1.000 0.000
#&gt; GSM49583     2  0.0000      0.998 0.000 1.000
#&gt; GSM49584     1  0.0000      0.985 1.000 0.000
#&gt; GSM49585     1  0.0000      0.985 1.000 0.000
#&gt; GSM49586     1  0.0000      0.985 1.000 0.000
#&gt; GSM49587     1  0.0000      0.985 1.000 0.000
#&gt; GSM49588     1  0.0000      0.985 1.000 0.000
#&gt; GSM49589     1  0.0000      0.985 1.000 0.000
#&gt; GSM49590     1  0.8144      0.670 0.748 0.252
#&gt; GSM49591     1  0.0000      0.985 1.000 0.000
#&gt; GSM49592     1  0.0000      0.985 1.000 0.000
#&gt; GSM49593     1  0.0000      0.985 1.000 0.000
#&gt; GSM49594     1  0.0000      0.985 1.000 0.000
#&gt; GSM49595     1  0.0000      0.985 1.000 0.000
#&gt; GSM49596     1  0.0000      0.985 1.000 0.000
#&gt; GSM49597     2  0.0000      0.998 0.000 1.000
#&gt; GSM49598     1  0.0000      0.985 1.000 0.000
#&gt; GSM49599     1  0.0000      0.985 1.000 0.000
#&gt; GSM49600     1  0.0000      0.985 1.000 0.000
#&gt; GSM49601     1  0.0000      0.985 1.000 0.000
#&gt; GSM49602     1  0.0000      0.985 1.000 0.000
#&gt; GSM49603     1  0.0000      0.985 1.000 0.000
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-1-a').click(function(){
  $('#tab-SD-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-2'>
<p><a id='tab-SD-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM49613     3  0.0000      0.755 0.000 0.000 1.000
#&gt; GSM49604     3  0.6225      0.471 0.000 0.432 0.568
#&gt; GSM49605     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM49606     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM49607     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM49608     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM49609     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM49610     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM49611     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM49612     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM49614     3  0.0000      0.755 0.000 0.000 1.000
#&gt; GSM49615     3  0.0000      0.755 0.000 0.000 1.000
#&gt; GSM49616     3  0.0000      0.755 0.000 0.000 1.000
#&gt; GSM49617     3  0.0000      0.755 0.000 0.000 1.000
#&gt; GSM49564     1  0.0237      0.994 0.996 0.000 0.004
#&gt; GSM49565     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49566     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49567     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49568     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49569     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49570     3  0.5560      0.631 0.000 0.300 0.700
#&gt; GSM49571     3  0.8645      0.420 0.300 0.132 0.568
#&gt; GSM49572     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49573     3  0.5882      0.588 0.000 0.348 0.652
#&gt; GSM49574     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49575     1  0.2066      0.933 0.940 0.000 0.060
#&gt; GSM49576     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49577     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49578     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49579     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49580     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49581     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49582     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49583     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM49584     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49585     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49586     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49587     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49588     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49589     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49590     1  0.0747      0.982 0.984 0.000 0.016
#&gt; GSM49591     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49592     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49593     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49594     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49595     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49596     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49597     3  0.6225      0.471 0.000 0.432 0.568
#&gt; GSM49598     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49599     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49600     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49601     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49602     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49603     1  0.0000      0.997 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-2-a').click(function(){
  $('#tab-SD-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-3'>
<p><a id='tab-SD-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM49613     3  0.0000      0.997 0.000 0.000 1.000 0.000
#&gt; GSM49604     4  0.4988      0.756 0.000 0.236 0.036 0.728
#&gt; GSM49605     2  0.0000      0.996 0.000 1.000 0.000 0.000
#&gt; GSM49606     2  0.0188      0.995 0.000 0.996 0.000 0.004
#&gt; GSM49607     2  0.0000      0.996 0.000 1.000 0.000 0.000
#&gt; GSM49608     2  0.0469      0.987 0.000 0.988 0.000 0.012
#&gt; GSM49609     2  0.0000      0.996 0.000 1.000 0.000 0.000
#&gt; GSM49610     2  0.0000      0.996 0.000 1.000 0.000 0.000
#&gt; GSM49611     2  0.0188      0.995 0.000 0.996 0.000 0.004
#&gt; GSM49612     2  0.0000      0.996 0.000 1.000 0.000 0.000
#&gt; GSM49614     3  0.0188      0.995 0.000 0.000 0.996 0.004
#&gt; GSM49615     3  0.0000      0.997 0.000 0.000 1.000 0.000
#&gt; GSM49616     3  0.0000      0.997 0.000 0.000 1.000 0.000
#&gt; GSM49617     3  0.0188      0.995 0.000 0.000 0.996 0.004
#&gt; GSM49564     1  0.5151      0.799 0.760 0.000 0.100 0.140
#&gt; GSM49565     1  0.1398      0.943 0.956 0.000 0.004 0.040
#&gt; GSM49566     1  0.2266      0.929 0.912 0.000 0.004 0.084
#&gt; GSM49567     1  0.1059      0.936 0.972 0.012 0.000 0.016
#&gt; GSM49568     1  0.0336      0.941 0.992 0.000 0.000 0.008
#&gt; GSM49569     1  0.2412      0.927 0.908 0.000 0.008 0.084
#&gt; GSM49570     4  0.4093      0.824 0.000 0.096 0.072 0.832
#&gt; GSM49571     4  0.6809      0.560 0.208 0.084 0.044 0.664
#&gt; GSM49572     1  0.0592      0.940 0.984 0.000 0.000 0.016
#&gt; GSM49573     4  0.3919      0.832 0.000 0.104 0.056 0.840
#&gt; GSM49574     1  0.0592      0.940 0.984 0.000 0.000 0.016
#&gt; GSM49575     1  0.1471      0.933 0.960 0.012 0.004 0.024
#&gt; GSM49576     1  0.5151      0.799 0.760 0.000 0.100 0.140
#&gt; GSM49577     1  0.1635      0.941 0.948 0.008 0.000 0.044
#&gt; GSM49578     1  0.0524      0.942 0.988 0.000 0.004 0.008
#&gt; GSM49579     1  0.2125      0.932 0.920 0.000 0.004 0.076
#&gt; GSM49580     1  0.1004      0.941 0.972 0.000 0.004 0.024
#&gt; GSM49581     1  0.0469      0.941 0.988 0.000 0.000 0.012
#&gt; GSM49582     1  0.0844      0.941 0.980 0.004 0.004 0.012
#&gt; GSM49583     2  0.0188      0.995 0.000 0.996 0.000 0.004
#&gt; GSM49584     1  0.1004      0.941 0.972 0.000 0.004 0.024
#&gt; GSM49585     1  0.0592      0.940 0.984 0.000 0.000 0.016
#&gt; GSM49586     1  0.2222      0.935 0.924 0.016 0.000 0.060
#&gt; GSM49587     1  0.0469      0.941 0.988 0.000 0.000 0.012
#&gt; GSM49588     1  0.0469      0.944 0.988 0.000 0.000 0.012
#&gt; GSM49589     1  0.5199      0.795 0.756 0.000 0.100 0.144
#&gt; GSM49590     1  0.5280      0.780 0.752 0.000 0.124 0.124
#&gt; GSM49591     1  0.0657      0.941 0.984 0.000 0.004 0.012
#&gt; GSM49592     1  0.0524      0.942 0.988 0.000 0.004 0.008
#&gt; GSM49593     1  0.2011      0.933 0.920 0.000 0.000 0.080
#&gt; GSM49594     1  0.1970      0.937 0.932 0.008 0.000 0.060
#&gt; GSM49595     1  0.1970      0.937 0.932 0.008 0.000 0.060
#&gt; GSM49596     1  0.1940      0.933 0.924 0.000 0.000 0.076
#&gt; GSM49597     4  0.4153      0.832 0.000 0.132 0.048 0.820
#&gt; GSM49598     1  0.1743      0.937 0.940 0.000 0.004 0.056
#&gt; GSM49599     1  0.1174      0.934 0.968 0.012 0.000 0.020
#&gt; GSM49600     1  0.1004      0.941 0.972 0.000 0.004 0.024
#&gt; GSM49601     1  0.1792      0.937 0.932 0.000 0.000 0.068
#&gt; GSM49602     1  0.2266      0.929 0.912 0.000 0.004 0.084
#&gt; GSM49603     1  0.2266      0.929 0.912 0.000 0.004 0.084
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-3-a').click(function(){
  $('#tab-SD-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-4'>
<p><a id='tab-SD-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2 p3    p4    p5
#&gt; GSM49613     3  0.0000      1.000 0.000 0.000  1 0.000 0.000
#&gt; GSM49604     4  0.0794      0.963 0.000 0.028  0 0.972 0.000
#&gt; GSM49605     2  0.0000      0.969 0.000 1.000  0 0.000 0.000
#&gt; GSM49606     2  0.0000      0.969 0.000 1.000  0 0.000 0.000
#&gt; GSM49607     2  0.0000      0.969 0.000 1.000  0 0.000 0.000
#&gt; GSM49608     2  0.1671      0.914 0.000 0.924  0 0.076 0.000
#&gt; GSM49609     2  0.0000      0.969 0.000 1.000  0 0.000 0.000
#&gt; GSM49610     2  0.0000      0.969 0.000 1.000  0 0.000 0.000
#&gt; GSM49611     2  0.0000      0.969 0.000 1.000  0 0.000 0.000
#&gt; GSM49612     2  0.0000      0.969 0.000 1.000  0 0.000 0.000
#&gt; GSM49614     3  0.0000      1.000 0.000 0.000  1 0.000 0.000
#&gt; GSM49615     3  0.0000      1.000 0.000 0.000  1 0.000 0.000
#&gt; GSM49616     3  0.0000      1.000 0.000 0.000  1 0.000 0.000
#&gt; GSM49617     3  0.0000      1.000 0.000 0.000  1 0.000 0.000
#&gt; GSM49564     5  0.0000      0.652 0.000 0.000  0 0.000 1.000
#&gt; GSM49565     1  0.3561      0.633 0.740 0.000  0 0.000 0.260
#&gt; GSM49566     5  0.1908      0.708 0.092 0.000  0 0.000 0.908
#&gt; GSM49567     1  0.2471      0.656 0.864 0.000  0 0.000 0.136
#&gt; GSM49568     5  0.4114      0.569 0.376 0.000  0 0.000 0.624
#&gt; GSM49569     5  0.1908      0.708 0.092 0.000  0 0.000 0.908
#&gt; GSM49570     4  0.0000      0.980 0.000 0.000  0 1.000 0.000
#&gt; GSM49571     1  0.3730      0.156 0.712 0.000  0 0.288 0.000
#&gt; GSM49572     1  0.3336      0.657 0.772 0.000  0 0.000 0.228
#&gt; GSM49573     4  0.0703      0.969 0.024 0.000  0 0.976 0.000
#&gt; GSM49574     1  0.3395      0.653 0.764 0.000  0 0.000 0.236
#&gt; GSM49575     1  0.0162      0.580 0.996 0.000  0 0.000 0.004
#&gt; GSM49576     5  0.0000      0.652 0.000 0.000  0 0.000 1.000
#&gt; GSM49577     1  0.3074      0.664 0.804 0.000  0 0.000 0.196
#&gt; GSM49578     5  0.3932      0.613 0.328 0.000  0 0.000 0.672
#&gt; GSM49579     5  0.1965      0.707 0.096 0.000  0 0.000 0.904
#&gt; GSM49580     5  0.4210      0.518 0.412 0.000  0 0.000 0.588
#&gt; GSM49581     1  0.3752      0.473 0.708 0.000  0 0.000 0.292
#&gt; GSM49582     5  0.4294      0.396 0.468 0.000  0 0.000 0.532
#&gt; GSM49583     2  0.2852      0.804 0.000 0.828  0 0.172 0.000
#&gt; GSM49584     5  0.3999      0.606 0.344 0.000  0 0.000 0.656
#&gt; GSM49585     1  0.4201      0.320 0.592 0.000  0 0.000 0.408
#&gt; GSM49586     1  0.2966      0.670 0.816 0.000  0 0.000 0.184
#&gt; GSM49587     5  0.4273      0.264 0.448 0.000  0 0.000 0.552
#&gt; GSM49588     1  0.4305     -0.183 0.512 0.000  0 0.000 0.488
#&gt; GSM49589     5  0.0000      0.652 0.000 0.000  0 0.000 1.000
#&gt; GSM49590     5  0.0000      0.652 0.000 0.000  0 0.000 1.000
#&gt; GSM49591     5  0.3895      0.620 0.320 0.000  0 0.000 0.680
#&gt; GSM49592     5  0.3913      0.618 0.324 0.000  0 0.000 0.676
#&gt; GSM49593     1  0.4307      0.362 0.504 0.000  0 0.000 0.496
#&gt; GSM49594     1  0.4262      0.493 0.560 0.000  0 0.000 0.440
#&gt; GSM49595     1  0.4268      0.488 0.556 0.000  0 0.000 0.444
#&gt; GSM49596     5  0.2329      0.698 0.124 0.000  0 0.000 0.876
#&gt; GSM49597     4  0.0000      0.980 0.000 0.000  0 1.000 0.000
#&gt; GSM49598     5  0.3480      0.661 0.248 0.000  0 0.000 0.752
#&gt; GSM49599     1  0.1544      0.627 0.932 0.000  0 0.000 0.068
#&gt; GSM49600     5  0.4101      0.577 0.372 0.000  0 0.000 0.628
#&gt; GSM49601     1  0.4249      0.466 0.568 0.000  0 0.000 0.432
#&gt; GSM49602     5  0.1908      0.708 0.092 0.000  0 0.000 0.908
#&gt; GSM49603     5  0.1908      0.708 0.092 0.000  0 0.000 0.908
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-4-a').click(function(){
  $('#tab-SD-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-5'>
<p><a id='tab-SD-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2 p3    p4    p5    p6
#&gt; GSM49613     3  0.0000     1.0000 0.000 0.000  1 0.000 0.000 0.000
#&gt; GSM49604     4  0.1682     0.9269 0.000 0.052  0 0.928 0.000 0.020
#&gt; GSM49605     2  0.0000     0.9352 0.000 1.000  0 0.000 0.000 0.000
#&gt; GSM49606     2  0.0937     0.9369 0.000 0.960  0 0.000 0.000 0.040
#&gt; GSM49607     2  0.0000     0.9352 0.000 1.000  0 0.000 0.000 0.000
#&gt; GSM49608     2  0.1471     0.8957 0.000 0.932  0 0.064 0.000 0.004
#&gt; GSM49609     2  0.0937     0.9369 0.000 0.960  0 0.000 0.000 0.040
#&gt; GSM49610     2  0.0000     0.9352 0.000 1.000  0 0.000 0.000 0.000
#&gt; GSM49611     2  0.0937     0.9369 0.000 0.960  0 0.000 0.000 0.040
#&gt; GSM49612     2  0.0937     0.9369 0.000 0.960  0 0.000 0.000 0.040
#&gt; GSM49614     3  0.0000     1.0000 0.000 0.000  1 0.000 0.000 0.000
#&gt; GSM49615     3  0.0000     1.0000 0.000 0.000  1 0.000 0.000 0.000
#&gt; GSM49616     3  0.0000     1.0000 0.000 0.000  1 0.000 0.000 0.000
#&gt; GSM49617     3  0.0000     1.0000 0.000 0.000  1 0.000 0.000 0.000
#&gt; GSM49564     5  0.0858     0.5757 0.004 0.000  0 0.000 0.968 0.028
#&gt; GSM49565     1  0.4587     0.4444 0.688 0.000  0 0.000 0.204 0.108
#&gt; GSM49566     5  0.3266     0.4724 0.272 0.000  0 0.000 0.728 0.000
#&gt; GSM49567     6  0.3867     0.3735 0.488 0.000  0 0.000 0.000 0.512
#&gt; GSM49568     1  0.4147     0.4082 0.716 0.000  0 0.000 0.224 0.060
#&gt; GSM49569     5  0.2912     0.5169 0.216 0.000  0 0.000 0.784 0.000
#&gt; GSM49570     4  0.0000     0.9599 0.000 0.000  0 1.000 0.000 0.000
#&gt; GSM49571     6  0.3841     0.0693 0.028 0.000  0 0.256 0.000 0.716
#&gt; GSM49572     1  0.3522     0.3853 0.800 0.000  0 0.000 0.072 0.128
#&gt; GSM49573     4  0.1075     0.9390 0.000 0.000  0 0.952 0.000 0.048
#&gt; GSM49574     1  0.3501     0.4248 0.804 0.000  0 0.000 0.080 0.116
#&gt; GSM49575     6  0.2703     0.4893 0.172 0.000  0 0.004 0.000 0.824
#&gt; GSM49576     5  0.0909     0.5885 0.020 0.000  0 0.000 0.968 0.012
#&gt; GSM49577     1  0.3956     0.2958 0.704 0.000  0 0.000 0.032 0.264
#&gt; GSM49578     1  0.4282     0.4853 0.720 0.000  0 0.000 0.192 0.088
#&gt; GSM49579     5  0.3838     0.1750 0.448 0.000  0 0.000 0.552 0.000
#&gt; GSM49580     1  0.5551     0.2173 0.556 0.000  0 0.000 0.220 0.224
#&gt; GSM49581     6  0.4408     0.1792 0.488 0.000  0 0.000 0.024 0.488
#&gt; GSM49582     1  0.5963     0.0541 0.476 0.000  0 0.004 0.216 0.304
#&gt; GSM49583     2  0.3371     0.5871 0.000 0.708  0 0.292 0.000 0.000
#&gt; GSM49584     1  0.5607     0.2260 0.544 0.000  0 0.000 0.240 0.216
#&gt; GSM49585     1  0.2398     0.5309 0.876 0.000  0 0.000 0.104 0.020
#&gt; GSM49586     6  0.4523     0.1581 0.452 0.000  0 0.000 0.032 0.516
#&gt; GSM49587     1  0.2706     0.5452 0.852 0.000  0 0.000 0.124 0.024
#&gt; GSM49588     1  0.1910     0.5458 0.892 0.000  0 0.000 0.108 0.000
#&gt; GSM49589     5  0.0622     0.5855 0.008 0.000  0 0.000 0.980 0.012
#&gt; GSM49590     5  0.0622     0.5855 0.008 0.000  0 0.000 0.980 0.012
#&gt; GSM49591     1  0.3645     0.4728 0.740 0.000  0 0.000 0.236 0.024
#&gt; GSM49592     1  0.3894     0.5052 0.760 0.000  0 0.000 0.168 0.072
#&gt; GSM49593     1  0.3534     0.3372 0.716 0.000  0 0.000 0.276 0.008
#&gt; GSM49594     1  0.4910     0.3643 0.640 0.000  0 0.000 0.116 0.244
#&gt; GSM49595     1  0.5027     0.4086 0.640 0.000  0 0.000 0.200 0.160
#&gt; GSM49596     1  0.3482     0.2938 0.684 0.000  0 0.000 0.316 0.000
#&gt; GSM49597     4  0.0000     0.9599 0.000 0.000  0 1.000 0.000 0.000
#&gt; GSM49598     5  0.5723    -0.0491 0.408 0.000  0 0.000 0.428 0.164
#&gt; GSM49599     6  0.3789     0.4641 0.416 0.000  0 0.000 0.000 0.584
#&gt; GSM49600     1  0.5747     0.2207 0.500 0.000  0 0.000 0.300 0.200
#&gt; GSM49601     1  0.4804     0.3432 0.656 0.000  0 0.000 0.112 0.232
#&gt; GSM49602     5  0.3860     0.1379 0.472 0.000  0 0.000 0.528 0.000
#&gt; GSM49603     5  0.3838     0.1859 0.448 0.000  0 0.000 0.552 0.000
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-5-a').click(function(){
  $('#tab-SD-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-SD-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-get-signatures'>
<ul>
<li><a href='#tab-SD-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-1-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-1"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-2-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-2"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-3-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-3"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-4-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-4"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-5-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-mclust-signature_compare](figure_cola/SD-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-SD-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-mclust-collect-classes](figure_cola/SD-mclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n disease.state(p) cell.type(p) k
#> SD:mclust 54         1.14e-05     4.79e-05 2
#> SD:mclust 51         3.42e-09     3.82e-11 3
#> SD:mclust 54         3.60e-08     3.28e-14 4
#> SD:mclust 44         4.64e-06     3.30e-10 5
#> SD:mclust 27         2.37e-03     2.02e-05 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:NMF*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "NMF"]
# you can also extract it by
# res = res_list["SD:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21168 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-NMF-collect-plots](figure_cola/SD-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-NMF-select-partition-number](figure_cola/SD-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.960           0.951       0.978         0.4081 0.609   0.609
#> 3 3 0.944           0.924       0.967         0.3976 0.821   0.706
#> 4 4 0.726           0.842       0.902         0.1439 0.969   0.927
#> 5 5 0.696           0.765       0.832         0.1242 0.819   0.555
#> 6 6 0.875           0.795       0.901         0.0767 0.938   0.755
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-get-classes'>
<ul>
<li><a href='#tab-SD-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-NMF-get-classes-1'>
<p><a id='tab-SD-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM49613     1  0.0000      0.971 1.000 0.000
#&gt; GSM49604     2  0.0000      0.993 0.000 1.000
#&gt; GSM49605     2  0.0000      0.993 0.000 1.000
#&gt; GSM49606     2  0.0000      0.993 0.000 1.000
#&gt; GSM49607     2  0.0000      0.993 0.000 1.000
#&gt; GSM49608     2  0.0000      0.993 0.000 1.000
#&gt; GSM49609     2  0.0000      0.993 0.000 1.000
#&gt; GSM49610     2  0.0000      0.993 0.000 1.000
#&gt; GSM49611     2  0.0000      0.993 0.000 1.000
#&gt; GSM49612     2  0.0000      0.993 0.000 1.000
#&gt; GSM49614     1  0.0000      0.971 1.000 0.000
#&gt; GSM49615     1  0.0000      0.971 1.000 0.000
#&gt; GSM49616     1  0.0000      0.971 1.000 0.000
#&gt; GSM49617     1  0.0000      0.971 1.000 0.000
#&gt; GSM49564     1  0.0000      0.971 1.000 0.000
#&gt; GSM49565     1  0.6623      0.797 0.828 0.172
#&gt; GSM49566     1  0.0000      0.971 1.000 0.000
#&gt; GSM49567     1  0.0000      0.971 1.000 0.000
#&gt; GSM49568     1  0.0000      0.971 1.000 0.000
#&gt; GSM49569     1  0.0000      0.971 1.000 0.000
#&gt; GSM49570     2  0.0938      0.984 0.012 0.988
#&gt; GSM49571     2  0.3584      0.923 0.068 0.932
#&gt; GSM49572     1  0.0000      0.971 1.000 0.000
#&gt; GSM49573     2  0.0000      0.993 0.000 1.000
#&gt; GSM49574     1  0.1414      0.956 0.980 0.020
#&gt; GSM49575     1  0.0000      0.971 1.000 0.000
#&gt; GSM49576     1  0.0000      0.971 1.000 0.000
#&gt; GSM49577     1  0.6801      0.787 0.820 0.180
#&gt; GSM49578     1  0.0000      0.971 1.000 0.000
#&gt; GSM49579     1  0.0000      0.971 1.000 0.000
#&gt; GSM49580     1  0.0000      0.971 1.000 0.000
#&gt; GSM49581     1  0.0000      0.971 1.000 0.000
#&gt; GSM49582     1  0.0000      0.971 1.000 0.000
#&gt; GSM49583     2  0.0000      0.993 0.000 1.000
#&gt; GSM49584     1  0.0000      0.971 1.000 0.000
#&gt; GSM49585     1  0.0000      0.971 1.000 0.000
#&gt; GSM49586     1  0.9209      0.526 0.664 0.336
#&gt; GSM49587     1  0.0000      0.971 1.000 0.000
#&gt; GSM49588     1  0.0000      0.971 1.000 0.000
#&gt; GSM49589     1  0.0000      0.971 1.000 0.000
#&gt; GSM49590     1  0.0000      0.971 1.000 0.000
#&gt; GSM49591     1  0.0000      0.971 1.000 0.000
#&gt; GSM49592     1  0.0000      0.971 1.000 0.000
#&gt; GSM49593     1  0.0000      0.971 1.000 0.000
#&gt; GSM49594     1  0.9323      0.502 0.652 0.348
#&gt; GSM49595     1  0.2948      0.928 0.948 0.052
#&gt; GSM49596     1  0.0000      0.971 1.000 0.000
#&gt; GSM49597     2  0.0000      0.993 0.000 1.000
#&gt; GSM49598     1  0.0000      0.971 1.000 0.000
#&gt; GSM49599     1  0.0000      0.971 1.000 0.000
#&gt; GSM49600     1  0.0000      0.971 1.000 0.000
#&gt; GSM49601     1  0.0000      0.971 1.000 0.000
#&gt; GSM49602     1  0.0000      0.971 1.000 0.000
#&gt; GSM49603     1  0.0000      0.971 1.000 0.000
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-1-a').click(function(){
  $('#tab-SD-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-2'>
<p><a id='tab-SD-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM49613     3  0.0424      0.957 0.008 0.000 0.992
#&gt; GSM49604     2  0.0000      0.930 0.000 1.000 0.000
#&gt; GSM49605     2  0.0237      0.931 0.000 0.996 0.004
#&gt; GSM49606     2  0.0237      0.931 0.000 0.996 0.004
#&gt; GSM49607     2  0.0237      0.931 0.000 0.996 0.004
#&gt; GSM49608     2  0.0424      0.929 0.000 0.992 0.008
#&gt; GSM49609     2  0.0000      0.930 0.000 1.000 0.000
#&gt; GSM49610     2  0.0424      0.929 0.000 0.992 0.008
#&gt; GSM49611     2  0.0000      0.930 0.000 1.000 0.000
#&gt; GSM49612     2  0.0000      0.930 0.000 1.000 0.000
#&gt; GSM49614     3  0.0424      0.952 0.000 0.008 0.992
#&gt; GSM49615     3  0.0237      0.957 0.004 0.000 0.996
#&gt; GSM49616     3  0.0237      0.957 0.004 0.000 0.996
#&gt; GSM49617     3  0.0424      0.952 0.000 0.008 0.992
#&gt; GSM49564     3  0.0592      0.955 0.012 0.000 0.988
#&gt; GSM49565     1  0.0661      0.968 0.988 0.008 0.004
#&gt; GSM49566     1  0.1411      0.956 0.964 0.000 0.036
#&gt; GSM49567     1  0.0661      0.968 0.988 0.008 0.004
#&gt; GSM49568     1  0.0237      0.971 0.996 0.000 0.004
#&gt; GSM49569     1  0.1964      0.939 0.944 0.000 0.056
#&gt; GSM49570     2  0.1031      0.918 0.000 0.976 0.024
#&gt; GSM49571     2  0.6520      0.031 0.488 0.508 0.004
#&gt; GSM49572     1  0.0661      0.968 0.988 0.008 0.004
#&gt; GSM49573     2  0.3293      0.825 0.088 0.900 0.012
#&gt; GSM49574     1  0.0661      0.968 0.988 0.008 0.004
#&gt; GSM49575     1  0.0661      0.968 0.988 0.008 0.004
#&gt; GSM49576     1  0.4931      0.710 0.768 0.000 0.232
#&gt; GSM49577     1  0.0983      0.964 0.980 0.016 0.004
#&gt; GSM49578     1  0.0000      0.971 1.000 0.000 0.000
#&gt; GSM49579     1  0.0237      0.971 0.996 0.000 0.004
#&gt; GSM49580     1  0.0747      0.967 0.984 0.000 0.016
#&gt; GSM49581     1  0.0000      0.971 1.000 0.000 0.000
#&gt; GSM49582     1  0.0237      0.971 0.996 0.000 0.004
#&gt; GSM49583     2  0.0000      0.930 0.000 1.000 0.000
#&gt; GSM49584     1  0.1031      0.963 0.976 0.000 0.024
#&gt; GSM49585     1  0.0000      0.971 1.000 0.000 0.000
#&gt; GSM49586     1  0.4351      0.799 0.828 0.168 0.004
#&gt; GSM49587     1  0.0000      0.971 1.000 0.000 0.000
#&gt; GSM49588     1  0.0237      0.971 0.996 0.000 0.004
#&gt; GSM49589     3  0.3941      0.794 0.156 0.000 0.844
#&gt; GSM49590     3  0.1643      0.931 0.044 0.000 0.956
#&gt; GSM49591     1  0.0237      0.970 0.996 0.000 0.004
#&gt; GSM49592     1  0.0000      0.971 1.000 0.000 0.000
#&gt; GSM49593     1  0.1163      0.961 0.972 0.000 0.028
#&gt; GSM49594     1  0.3112      0.888 0.900 0.096 0.004
#&gt; GSM49595     1  0.0661      0.968 0.988 0.008 0.004
#&gt; GSM49596     1  0.0237      0.971 0.996 0.000 0.004
#&gt; GSM49597     2  0.0424      0.929 0.000 0.992 0.008
#&gt; GSM49598     1  0.0892      0.965 0.980 0.000 0.020
#&gt; GSM49599     1  0.0661      0.968 0.988 0.008 0.004
#&gt; GSM49600     1  0.1411      0.956 0.964 0.000 0.036
#&gt; GSM49601     1  0.0237      0.971 0.996 0.000 0.004
#&gt; GSM49602     1  0.0424      0.970 0.992 0.000 0.008
#&gt; GSM49603     1  0.1163      0.961 0.972 0.000 0.028
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-2-a').click(function(){
  $('#tab-SD-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-3'>
<p><a id='tab-SD-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM49613     3  0.0188     0.9368 0.000 0.000 0.996 0.004
#&gt; GSM49604     4  0.4040     0.7346 0.000 0.248 0.000 0.752
#&gt; GSM49605     2  0.0336     0.9903 0.000 0.992 0.000 0.008
#&gt; GSM49606     2  0.0000     0.9899 0.000 1.000 0.000 0.000
#&gt; GSM49607     2  0.0592     0.9862 0.000 0.984 0.000 0.016
#&gt; GSM49608     2  0.0336     0.9903 0.000 0.992 0.000 0.008
#&gt; GSM49609     2  0.0000     0.9899 0.000 1.000 0.000 0.000
#&gt; GSM49610     2  0.0469     0.9889 0.000 0.988 0.000 0.012
#&gt; GSM49611     2  0.0000     0.9899 0.000 1.000 0.000 0.000
#&gt; GSM49612     2  0.0000     0.9899 0.000 1.000 0.000 0.000
#&gt; GSM49614     3  0.2216     0.8726 0.000 0.000 0.908 0.092
#&gt; GSM49615     3  0.0000     0.9368 0.000 0.000 1.000 0.000
#&gt; GSM49616     3  0.0817     0.9307 0.000 0.000 0.976 0.024
#&gt; GSM49617     3  0.1557     0.9104 0.000 0.000 0.944 0.056
#&gt; GSM49564     3  0.0469     0.9345 0.000 0.000 0.988 0.012
#&gt; GSM49565     1  0.1059     0.8733 0.972 0.012 0.000 0.016
#&gt; GSM49566     1  0.4227     0.8211 0.820 0.000 0.120 0.060
#&gt; GSM49567     1  0.1792     0.8564 0.932 0.000 0.000 0.068
#&gt; GSM49568     1  0.1305     0.8688 0.960 0.000 0.004 0.036
#&gt; GSM49569     1  0.4046     0.8247 0.828 0.000 0.124 0.048
#&gt; GSM49570     4  0.2329     0.8319 0.000 0.072 0.012 0.916
#&gt; GSM49571     4  0.3427     0.7594 0.112 0.028 0.000 0.860
#&gt; GSM49572     1  0.0336     0.8724 0.992 0.000 0.000 0.008
#&gt; GSM49573     4  0.2036     0.8239 0.032 0.032 0.000 0.936
#&gt; GSM49574     1  0.0817     0.8720 0.976 0.000 0.000 0.024
#&gt; GSM49575     1  0.5000     0.0449 0.504 0.000 0.000 0.496
#&gt; GSM49576     1  0.5721     0.6428 0.660 0.000 0.284 0.056
#&gt; GSM49577     1  0.1209     0.8714 0.964 0.004 0.000 0.032
#&gt; GSM49578     1  0.1118     0.8671 0.964 0.000 0.000 0.036
#&gt; GSM49579     1  0.3885     0.8394 0.844 0.000 0.064 0.092
#&gt; GSM49580     1  0.1978     0.8584 0.928 0.000 0.004 0.068
#&gt; GSM49581     1  0.2345     0.8365 0.900 0.000 0.000 0.100
#&gt; GSM49582     1  0.1637     0.8599 0.940 0.000 0.000 0.060
#&gt; GSM49583     2  0.0921     0.9754 0.000 0.972 0.000 0.028
#&gt; GSM49584     1  0.1706     0.8723 0.948 0.000 0.016 0.036
#&gt; GSM49585     1  0.0376     0.8730 0.992 0.000 0.004 0.004
#&gt; GSM49586     1  0.5769     0.4221 0.588 0.036 0.000 0.376
#&gt; GSM49587     1  0.1059     0.8737 0.972 0.000 0.012 0.016
#&gt; GSM49588     1  0.1724     0.8708 0.948 0.000 0.032 0.020
#&gt; GSM49589     3  0.1975     0.8860 0.048 0.000 0.936 0.016
#&gt; GSM49590     3  0.2500     0.8736 0.040 0.000 0.916 0.044
#&gt; GSM49591     1  0.0707     0.8711 0.980 0.000 0.000 0.020
#&gt; GSM49592     1  0.0921     0.8692 0.972 0.000 0.000 0.028
#&gt; GSM49593     1  0.3840     0.8337 0.844 0.000 0.104 0.052
#&gt; GSM49594     1  0.5180     0.7195 0.740 0.196 0.000 0.064
#&gt; GSM49595     1  0.4155     0.8279 0.840 0.084 0.008 0.068
#&gt; GSM49596     1  0.2411     0.8624 0.920 0.000 0.040 0.040
#&gt; GSM49597     4  0.4198     0.7387 0.004 0.224 0.004 0.768
#&gt; GSM49598     1  0.2589     0.8578 0.884 0.000 0.000 0.116
#&gt; GSM49599     1  0.4605     0.5165 0.664 0.000 0.000 0.336
#&gt; GSM49600     1  0.1733     0.8744 0.948 0.000 0.028 0.024
#&gt; GSM49601     1  0.4147     0.8330 0.840 0.008 0.088 0.064
#&gt; GSM49602     1  0.4469     0.8192 0.808 0.000 0.080 0.112
#&gt; GSM49603     1  0.4786     0.8057 0.788 0.000 0.104 0.108
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-3-a').click(function(){
  $('#tab-SD-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-4'>
<p><a id='tab-SD-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM49613     3  0.0290     0.9396 0.000 0.000 0.992 0.008 0.000
#&gt; GSM49604     4  0.2388     0.8205 0.000 0.072 0.000 0.900 0.028
#&gt; GSM49605     2  0.0162     0.9308 0.000 0.996 0.000 0.000 0.004
#&gt; GSM49606     2  0.0000     0.9309 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49607     2  0.2563     0.8749 0.000 0.872 0.000 0.008 0.120
#&gt; GSM49608     2  0.1704     0.9071 0.000 0.928 0.000 0.004 0.068
#&gt; GSM49609     2  0.1270     0.9116 0.000 0.948 0.000 0.000 0.052
#&gt; GSM49610     2  0.0324     0.9309 0.000 0.992 0.000 0.004 0.004
#&gt; GSM49611     2  0.1043     0.9198 0.000 0.960 0.000 0.000 0.040
#&gt; GSM49612     2  0.0609     0.9279 0.000 0.980 0.000 0.000 0.020
#&gt; GSM49614     3  0.1211     0.9169 0.000 0.000 0.960 0.016 0.024
#&gt; GSM49615     3  0.0162     0.9399 0.000 0.000 0.996 0.004 0.000
#&gt; GSM49616     3  0.0162     0.9407 0.000 0.000 0.996 0.004 0.000
#&gt; GSM49617     3  0.0162     0.9407 0.000 0.000 0.996 0.004 0.000
#&gt; GSM49564     3  0.0000     0.9408 0.000 0.000 1.000 0.000 0.000
#&gt; GSM49565     1  0.0609     0.8901 0.980 0.000 0.000 0.000 0.020
#&gt; GSM49566     5  0.4821     0.5300 0.464 0.000 0.020 0.000 0.516
#&gt; GSM49567     1  0.1270     0.8664 0.948 0.000 0.000 0.000 0.052
#&gt; GSM49568     1  0.0404     0.8914 0.988 0.000 0.000 0.000 0.012
#&gt; GSM49569     5  0.5201     0.6526 0.424 0.000 0.044 0.000 0.532
#&gt; GSM49570     4  0.2233     0.8199 0.000 0.000 0.004 0.892 0.104
#&gt; GSM49571     4  0.2228     0.8187 0.012 0.000 0.004 0.908 0.076
#&gt; GSM49572     1  0.0880     0.8834 0.968 0.000 0.000 0.000 0.032
#&gt; GSM49573     4  0.0579     0.8352 0.008 0.000 0.000 0.984 0.008
#&gt; GSM49574     1  0.0000     0.8926 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49575     1  0.3513     0.6744 0.800 0.000 0.000 0.180 0.020
#&gt; GSM49576     5  0.6008     0.7103 0.292 0.000 0.148 0.000 0.560
#&gt; GSM49577     1  0.4387     0.1916 0.640 0.012 0.000 0.000 0.348
#&gt; GSM49578     1  0.0162     0.8919 0.996 0.000 0.000 0.000 0.004
#&gt; GSM49579     5  0.4420     0.6107 0.448 0.000 0.004 0.000 0.548
#&gt; GSM49580     1  0.0898     0.8899 0.972 0.000 0.000 0.008 0.020
#&gt; GSM49581     1  0.0510     0.8915 0.984 0.000 0.000 0.000 0.016
#&gt; GSM49582     1  0.0290     0.8909 0.992 0.000 0.000 0.000 0.008
#&gt; GSM49583     2  0.4240     0.7400 0.004 0.732 0.000 0.024 0.240
#&gt; GSM49584     1  0.0854     0.8906 0.976 0.000 0.008 0.004 0.012
#&gt; GSM49585     1  0.2408     0.8154 0.892 0.000 0.004 0.008 0.096
#&gt; GSM49586     5  0.4826    -0.2298 0.020 0.000 0.000 0.472 0.508
#&gt; GSM49587     1  0.0000     0.8926 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49588     1  0.1557     0.8614 0.940 0.000 0.008 0.000 0.052
#&gt; GSM49589     3  0.3430     0.6764 0.004 0.000 0.776 0.000 0.220
#&gt; GSM49590     5  0.4425     0.0439 0.004 0.000 0.452 0.000 0.544
#&gt; GSM49591     1  0.0609     0.8902 0.980 0.000 0.000 0.000 0.020
#&gt; GSM49592     1  0.0671     0.8898 0.980 0.000 0.000 0.004 0.016
#&gt; GSM49593     5  0.4551     0.7213 0.368 0.000 0.016 0.000 0.616
#&gt; GSM49594     5  0.4930     0.7314 0.244 0.072 0.000 0.000 0.684
#&gt; GSM49595     5  0.4437     0.7453 0.316 0.020 0.000 0.000 0.664
#&gt; GSM49596     1  0.4251    -0.0983 0.624 0.000 0.004 0.000 0.372
#&gt; GSM49597     4  0.5906     0.5331 0.000 0.104 0.000 0.492 0.404
#&gt; GSM49598     5  0.4675     0.7021 0.380 0.000 0.000 0.020 0.600
#&gt; GSM49599     1  0.2570     0.8268 0.888 0.000 0.000 0.084 0.028
#&gt; GSM49600     1  0.1569     0.8676 0.944 0.000 0.008 0.004 0.044
#&gt; GSM49601     5  0.5079     0.7247 0.232 0.028 0.040 0.000 0.700
#&gt; GSM49602     5  0.4309     0.7487 0.308 0.000 0.016 0.000 0.676
#&gt; GSM49603     5  0.5059     0.7329 0.224 0.000 0.052 0.020 0.704
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-4-a').click(function(){
  $('#tab-SD-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-5'>
<p><a id='tab-SD-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM49613     3  0.0405     0.9036 0.000 0.000 0.988 0.000 0.008 0.004
#&gt; GSM49604     4  0.3313     0.6178 0.000 0.124 0.000 0.816 0.000 0.060
#&gt; GSM49605     2  0.1663     0.8511 0.000 0.912 0.000 0.000 0.000 0.088
#&gt; GSM49606     2  0.0937     0.8730 0.000 0.960 0.000 0.000 0.000 0.040
#&gt; GSM49607     2  0.2941     0.6638 0.000 0.780 0.000 0.000 0.000 0.220
#&gt; GSM49608     2  0.2355     0.8211 0.000 0.876 0.008 0.000 0.004 0.112
#&gt; GSM49609     2  0.1806     0.8418 0.000 0.928 0.000 0.020 0.008 0.044
#&gt; GSM49610     2  0.0146     0.8777 0.000 0.996 0.000 0.004 0.000 0.000
#&gt; GSM49611     2  0.1196     0.8604 0.000 0.952 0.000 0.008 0.000 0.040
#&gt; GSM49612     2  0.0837     0.8725 0.000 0.972 0.000 0.004 0.004 0.020
#&gt; GSM49614     3  0.1152     0.8721 0.000 0.000 0.952 0.004 0.000 0.044
#&gt; GSM49615     3  0.0260     0.9046 0.000 0.000 0.992 0.000 0.008 0.000
#&gt; GSM49616     3  0.0260     0.9046 0.000 0.000 0.992 0.000 0.008 0.000
#&gt; GSM49617     3  0.0405     0.9006 0.000 0.000 0.988 0.000 0.004 0.008
#&gt; GSM49564     3  0.0603     0.8999 0.000 0.000 0.980 0.000 0.016 0.004
#&gt; GSM49565     1  0.0806     0.9572 0.972 0.000 0.000 0.000 0.008 0.020
#&gt; GSM49566     5  0.3718     0.7389 0.084 0.000 0.000 0.000 0.784 0.132
#&gt; GSM49567     1  0.0603     0.9604 0.980 0.000 0.000 0.004 0.000 0.016
#&gt; GSM49568     1  0.0000     0.9628 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM49569     5  0.1737     0.8552 0.040 0.000 0.008 0.000 0.932 0.020
#&gt; GSM49570     4  0.3979     0.4592 0.000 0.000 0.004 0.540 0.000 0.456
#&gt; GSM49571     4  0.0551     0.6608 0.004 0.004 0.000 0.984 0.008 0.000
#&gt; GSM49572     1  0.0363     0.9623 0.988 0.000 0.000 0.000 0.000 0.012
#&gt; GSM49573     4  0.3221     0.6493 0.004 0.000 0.004 0.772 0.000 0.220
#&gt; GSM49574     1  0.0291     0.9629 0.992 0.000 0.004 0.000 0.000 0.004
#&gt; GSM49575     1  0.2632     0.8254 0.832 0.000 0.000 0.164 0.000 0.004
#&gt; GSM49576     5  0.0972     0.8590 0.008 0.000 0.000 0.000 0.964 0.028
#&gt; GSM49577     5  0.6465     0.0963 0.336 0.024 0.000 0.000 0.412 0.228
#&gt; GSM49578     1  0.0000     0.9628 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM49579     5  0.2106     0.8380 0.032 0.000 0.000 0.000 0.904 0.064
#&gt; GSM49580     1  0.0653     0.9616 0.980 0.000 0.000 0.004 0.004 0.012
#&gt; GSM49581     1  0.0000     0.9628 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM49582     1  0.0146     0.9628 0.996 0.000 0.000 0.004 0.000 0.000
#&gt; GSM49583     6  0.3999    -0.1785 0.000 0.496 0.000 0.000 0.004 0.500
#&gt; GSM49584     1  0.0405     0.9628 0.988 0.000 0.000 0.004 0.000 0.008
#&gt; GSM49585     1  0.3282     0.8592 0.844 0.000 0.004 0.096 0.020 0.036
#&gt; GSM49586     4  0.4561     0.4160 0.000 0.016 0.004 0.676 0.272 0.032
#&gt; GSM49587     1  0.0260     0.9630 0.992 0.000 0.000 0.000 0.000 0.008
#&gt; GSM49588     1  0.2051     0.9332 0.924 0.000 0.012 0.020 0.032 0.012
#&gt; GSM49589     3  0.3728     0.4526 0.000 0.000 0.652 0.004 0.344 0.000
#&gt; GSM49590     5  0.1334     0.8522 0.000 0.000 0.032 0.000 0.948 0.020
#&gt; GSM49591     1  0.1116     0.9510 0.960 0.000 0.000 0.028 0.004 0.008
#&gt; GSM49592     1  0.1478     0.9444 0.944 0.000 0.004 0.032 0.000 0.020
#&gt; GSM49593     5  0.0870     0.8599 0.012 0.000 0.012 0.004 0.972 0.000
#&gt; GSM49594     5  0.0692     0.8574 0.000 0.004 0.000 0.000 0.976 0.020
#&gt; GSM49595     5  0.0665     0.8603 0.008 0.000 0.000 0.008 0.980 0.004
#&gt; GSM49596     5  0.3565     0.5888 0.276 0.000 0.000 0.004 0.716 0.004
#&gt; GSM49597     6  0.3089     0.1219 0.000 0.040 0.000 0.024 0.080 0.856
#&gt; GSM49598     5  0.1440     0.8568 0.032 0.000 0.004 0.012 0.948 0.004
#&gt; GSM49599     1  0.0692     0.9590 0.976 0.000 0.000 0.020 0.000 0.004
#&gt; GSM49600     1  0.1621     0.9287 0.936 0.000 0.008 0.004 0.048 0.004
#&gt; GSM49601     5  0.4324     0.7217 0.008 0.020 0.036 0.088 0.800 0.048
#&gt; GSM49602     5  0.0436     0.8590 0.004 0.000 0.000 0.004 0.988 0.004
#&gt; GSM49603     5  0.1129     0.8543 0.004 0.000 0.012 0.012 0.964 0.008
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-5-a').click(function(){
  $('#tab-SD-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-SD-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-get-signatures'>
<ul>
<li><a href='#tab-SD-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-1-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-1"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-2-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-2"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-3-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-3"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-4-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-4"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-5-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-NMF-signature_compare](figure_cola/SD-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-SD-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-NMF-collect-classes](figure_cola/SD-NMF-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>         n disease.state(p) cell.type(p) k
#> SD:NMF 54         2.33e-06     1.17e-03 2
#> SD:NMF 53         4.58e-07     6.93e-09 3
#> SD:NMF 52         9.58e-08     3.47e-09 4
#> SD:NMF 50         7.15e-07     8.69e-09 5
#> SD:NMF 48         7.29e-08     3.70e-10 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:hclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "hclust"]
# you can also extract it by
# res = res_list["CV:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21168 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 4.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-hclust-collect-plots](figure_cola/CV-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-hclust-select-partition-number](figure_cola/CV-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.866           0.921       0.958         0.3676 0.628   0.628
#> 3 3 0.870           0.914       0.968         0.3520 0.874   0.800
#> 4 4 0.991           0.927       0.973         0.0937 0.948   0.896
#> 5 5 0.803           0.844       0.907         0.0797 0.994   0.988
#> 6 6 0.589           0.733       0.815         0.0989 0.971   0.935
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 4
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-get-classes'>
<ul>
<li><a href='#tab-CV-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-hclust-get-classes-1'>
<p><a id='tab-CV-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM49613     1  0.2778      0.929 0.952 0.048
#&gt; GSM49604     2  0.6048      0.891 0.148 0.852
#&gt; GSM49605     2  0.2778      0.950 0.048 0.952
#&gt; GSM49606     2  0.2778      0.950 0.048 0.952
#&gt; GSM49607     2  0.2778      0.950 0.048 0.952
#&gt; GSM49608     2  0.2778      0.950 0.048 0.952
#&gt; GSM49609     2  0.2778      0.950 0.048 0.952
#&gt; GSM49610     2  0.2778      0.950 0.048 0.952
#&gt; GSM49611     2  0.2778      0.950 0.048 0.952
#&gt; GSM49612     2  0.2778      0.950 0.048 0.952
#&gt; GSM49614     1  0.2778      0.929 0.952 0.048
#&gt; GSM49615     1  0.2778      0.929 0.952 0.048
#&gt; GSM49616     1  0.2778      0.929 0.952 0.048
#&gt; GSM49617     1  0.2778      0.929 0.952 0.048
#&gt; GSM49564     1  0.0000      0.968 1.000 0.000
#&gt; GSM49565     1  0.0000      0.968 1.000 0.000
#&gt; GSM49566     1  0.0000      0.968 1.000 0.000
#&gt; GSM49567     1  0.0000      0.968 1.000 0.000
#&gt; GSM49568     1  0.0000      0.968 1.000 0.000
#&gt; GSM49569     1  0.0000      0.968 1.000 0.000
#&gt; GSM49570     2  0.7674      0.807 0.224 0.776
#&gt; GSM49571     1  0.9996     -0.129 0.512 0.488
#&gt; GSM49572     1  0.0000      0.968 1.000 0.000
#&gt; GSM49573     2  0.7674      0.807 0.224 0.776
#&gt; GSM49574     1  0.0000      0.968 1.000 0.000
#&gt; GSM49575     1  0.0000      0.968 1.000 0.000
#&gt; GSM49576     1  0.0000      0.968 1.000 0.000
#&gt; GSM49577     1  0.0000      0.968 1.000 0.000
#&gt; GSM49578     1  0.0000      0.968 1.000 0.000
#&gt; GSM49579     1  0.0000      0.968 1.000 0.000
#&gt; GSM49580     1  0.0000      0.968 1.000 0.000
#&gt; GSM49581     1  0.0000      0.968 1.000 0.000
#&gt; GSM49582     1  0.0000      0.968 1.000 0.000
#&gt; GSM49583     2  0.2778      0.950 0.048 0.952
#&gt; GSM49584     1  0.0000      0.968 1.000 0.000
#&gt; GSM49585     1  0.0000      0.968 1.000 0.000
#&gt; GSM49586     1  0.9460      0.331 0.636 0.364
#&gt; GSM49587     1  0.0000      0.968 1.000 0.000
#&gt; GSM49588     1  0.0000      0.968 1.000 0.000
#&gt; GSM49589     1  0.0000      0.968 1.000 0.000
#&gt; GSM49590     1  0.0000      0.968 1.000 0.000
#&gt; GSM49591     1  0.0000      0.968 1.000 0.000
#&gt; GSM49592     1  0.0000      0.968 1.000 0.000
#&gt; GSM49593     1  0.0000      0.968 1.000 0.000
#&gt; GSM49594     1  0.0938      0.958 0.988 0.012
#&gt; GSM49595     1  0.0938      0.958 0.988 0.012
#&gt; GSM49596     1  0.0000      0.968 1.000 0.000
#&gt; GSM49597     2  0.5842      0.897 0.140 0.860
#&gt; GSM49598     1  0.0000      0.968 1.000 0.000
#&gt; GSM49599     1  0.0000      0.968 1.000 0.000
#&gt; GSM49600     1  0.0000      0.968 1.000 0.000
#&gt; GSM49601     1  0.0000      0.968 1.000 0.000
#&gt; GSM49602     1  0.0000      0.968 1.000 0.000
#&gt; GSM49603     1  0.0000      0.968 1.000 0.000
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-1-a').click(function(){
  $('#tab-CV-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-2'>
<p><a id='tab-CV-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM49613     3  0.0747     1.0000 0.016 0.000 0.984
#&gt; GSM49604     2  0.4128     0.7969 0.132 0.856 0.012
#&gt; GSM49605     2  0.0000     0.8962 0.000 1.000 0.000
#&gt; GSM49606     2  0.0000     0.8962 0.000 1.000 0.000
#&gt; GSM49607     2  0.0000     0.8962 0.000 1.000 0.000
#&gt; GSM49608     2  0.0000     0.8962 0.000 1.000 0.000
#&gt; GSM49609     2  0.0000     0.8962 0.000 1.000 0.000
#&gt; GSM49610     2  0.0000     0.8962 0.000 1.000 0.000
#&gt; GSM49611     2  0.0000     0.8962 0.000 1.000 0.000
#&gt; GSM49612     2  0.0000     0.8962 0.000 1.000 0.000
#&gt; GSM49614     3  0.0747     1.0000 0.016 0.000 0.984
#&gt; GSM49615     3  0.0747     1.0000 0.016 0.000 0.984
#&gt; GSM49616     3  0.0747     1.0000 0.016 0.000 0.984
#&gt; GSM49617     3  0.0747     1.0000 0.016 0.000 0.984
#&gt; GSM49564     1  0.0000     0.9721 1.000 0.000 0.000
#&gt; GSM49565     1  0.0000     0.9721 1.000 0.000 0.000
#&gt; GSM49566     1  0.0000     0.9721 1.000 0.000 0.000
#&gt; GSM49567     1  0.0000     0.9721 1.000 0.000 0.000
#&gt; GSM49568     1  0.0000     0.9721 1.000 0.000 0.000
#&gt; GSM49569     1  0.0000     0.9721 1.000 0.000 0.000
#&gt; GSM49570     2  0.5551     0.6844 0.224 0.760 0.016
#&gt; GSM49571     1  0.6819    -0.0567 0.512 0.476 0.012
#&gt; GSM49572     1  0.0000     0.9721 1.000 0.000 0.000
#&gt; GSM49573     2  0.5551     0.6844 0.224 0.760 0.016
#&gt; GSM49574     1  0.0000     0.9721 1.000 0.000 0.000
#&gt; GSM49575     1  0.0000     0.9721 1.000 0.000 0.000
#&gt; GSM49576     1  0.0000     0.9721 1.000 0.000 0.000
#&gt; GSM49577     1  0.0000     0.9721 1.000 0.000 0.000
#&gt; GSM49578     1  0.0000     0.9721 1.000 0.000 0.000
#&gt; GSM49579     1  0.0000     0.9721 1.000 0.000 0.000
#&gt; GSM49580     1  0.0000     0.9721 1.000 0.000 0.000
#&gt; GSM49581     1  0.0000     0.9721 1.000 0.000 0.000
#&gt; GSM49582     1  0.0000     0.9721 1.000 0.000 0.000
#&gt; GSM49583     2  0.0661     0.8915 0.008 0.988 0.004
#&gt; GSM49584     1  0.0000     0.9721 1.000 0.000 0.000
#&gt; GSM49585     1  0.0000     0.9721 1.000 0.000 0.000
#&gt; GSM49586     1  0.6318     0.3689 0.636 0.356 0.008
#&gt; GSM49587     1  0.0000     0.9721 1.000 0.000 0.000
#&gt; GSM49588     1  0.0000     0.9721 1.000 0.000 0.000
#&gt; GSM49589     1  0.0000     0.9721 1.000 0.000 0.000
#&gt; GSM49590     1  0.0000     0.9721 1.000 0.000 0.000
#&gt; GSM49591     1  0.0000     0.9721 1.000 0.000 0.000
#&gt; GSM49592     1  0.0000     0.9721 1.000 0.000 0.000
#&gt; GSM49593     1  0.0000     0.9721 1.000 0.000 0.000
#&gt; GSM49594     1  0.0661     0.9613 0.988 0.008 0.004
#&gt; GSM49595     1  0.0661     0.9613 0.988 0.008 0.004
#&gt; GSM49596     1  0.0000     0.9721 1.000 0.000 0.000
#&gt; GSM49597     2  0.4261     0.7889 0.140 0.848 0.012
#&gt; GSM49598     1  0.0000     0.9721 1.000 0.000 0.000
#&gt; GSM49599     1  0.0000     0.9721 1.000 0.000 0.000
#&gt; GSM49600     1  0.0000     0.9721 1.000 0.000 0.000
#&gt; GSM49601     1  0.0000     0.9721 1.000 0.000 0.000
#&gt; GSM49602     1  0.0000     0.9721 1.000 0.000 0.000
#&gt; GSM49603     1  0.0000     0.9721 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-2-a').click(function(){
  $('#tab-CV-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-3'>
<p><a id='tab-CV-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2 p3    p4
#&gt; GSM49613     3  0.0000      1.000 0.000 0.000  1 0.000
#&gt; GSM49604     4  0.4331      0.505 0.000 0.288  0 0.712
#&gt; GSM49605     2  0.0000      0.996 0.000 1.000  0 0.000
#&gt; GSM49606     2  0.0000      0.996 0.000 1.000  0 0.000
#&gt; GSM49607     2  0.0000      0.996 0.000 1.000  0 0.000
#&gt; GSM49608     2  0.0000      0.996 0.000 1.000  0 0.000
#&gt; GSM49609     2  0.0000      0.996 0.000 1.000  0 0.000
#&gt; GSM49610     2  0.0000      0.996 0.000 1.000  0 0.000
#&gt; GSM49611     2  0.0000      0.996 0.000 1.000  0 0.000
#&gt; GSM49612     2  0.0000      0.996 0.000 1.000  0 0.000
#&gt; GSM49614     3  0.0000      1.000 0.000 0.000  1 0.000
#&gt; GSM49615     3  0.0000      1.000 0.000 0.000  1 0.000
#&gt; GSM49616     3  0.0000      1.000 0.000 0.000  1 0.000
#&gt; GSM49617     3  0.0000      1.000 0.000 0.000  1 0.000
#&gt; GSM49564     1  0.0000      0.985 1.000 0.000  0 0.000
#&gt; GSM49565     1  0.0000      0.985 1.000 0.000  0 0.000
#&gt; GSM49566     1  0.0000      0.985 1.000 0.000  0 0.000
#&gt; GSM49567     1  0.0000      0.985 1.000 0.000  0 0.000
#&gt; GSM49568     1  0.0000      0.985 1.000 0.000  0 0.000
#&gt; GSM49569     1  0.0000      0.985 1.000 0.000  0 0.000
#&gt; GSM49570     4  0.0000      0.615 0.000 0.000  0 1.000
#&gt; GSM49571     4  0.5172      0.286 0.404 0.008  0 0.588
#&gt; GSM49572     1  0.0000      0.985 1.000 0.000  0 0.000
#&gt; GSM49573     4  0.0000      0.615 0.000 0.000  0 1.000
#&gt; GSM49574     1  0.0000      0.985 1.000 0.000  0 0.000
#&gt; GSM49575     1  0.0000      0.985 1.000 0.000  0 0.000
#&gt; GSM49576     1  0.0000      0.985 1.000 0.000  0 0.000
#&gt; GSM49577     1  0.0000      0.985 1.000 0.000  0 0.000
#&gt; GSM49578     1  0.0000      0.985 1.000 0.000  0 0.000
#&gt; GSM49579     1  0.0000      0.985 1.000 0.000  0 0.000
#&gt; GSM49580     1  0.0000      0.985 1.000 0.000  0 0.000
#&gt; GSM49581     1  0.0000      0.985 1.000 0.000  0 0.000
#&gt; GSM49582     1  0.0000      0.985 1.000 0.000  0 0.000
#&gt; GSM49583     2  0.0921      0.969 0.000 0.972  0 0.028
#&gt; GSM49584     1  0.0000      0.985 1.000 0.000  0 0.000
#&gt; GSM49585     1  0.0000      0.985 1.000 0.000  0 0.000
#&gt; GSM49586     1  0.5212      0.128 0.572 0.008  0 0.420
#&gt; GSM49587     1  0.0000      0.985 1.000 0.000  0 0.000
#&gt; GSM49588     1  0.0000      0.985 1.000 0.000  0 0.000
#&gt; GSM49589     1  0.0000      0.985 1.000 0.000  0 0.000
#&gt; GSM49590     1  0.0000      0.985 1.000 0.000  0 0.000
#&gt; GSM49591     1  0.0000      0.985 1.000 0.000  0 0.000
#&gt; GSM49592     1  0.0000      0.985 1.000 0.000  0 0.000
#&gt; GSM49593     1  0.0000      0.985 1.000 0.000  0 0.000
#&gt; GSM49594     1  0.0469      0.973 0.988 0.000  0 0.012
#&gt; GSM49595     1  0.0469      0.973 0.988 0.000  0 0.012
#&gt; GSM49596     1  0.0000      0.985 1.000 0.000  0 0.000
#&gt; GSM49597     4  0.4277      0.493 0.000 0.280  0 0.720
#&gt; GSM49598     1  0.0000      0.985 1.000 0.000  0 0.000
#&gt; GSM49599     1  0.0000      0.985 1.000 0.000  0 0.000
#&gt; GSM49600     1  0.0000      0.985 1.000 0.000  0 0.000
#&gt; GSM49601     1  0.0000      0.985 1.000 0.000  0 0.000
#&gt; GSM49602     1  0.0000      0.985 1.000 0.000  0 0.000
#&gt; GSM49603     1  0.0000      0.985 1.000 0.000  0 0.000
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-3-a').click(function(){
  $('#tab-CV-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-4'>
<p><a id='tab-CV-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2 p3    p4    p5
#&gt; GSM49613     3   0.000    1.00000 0.000 0.000  1 0.000 0.000
#&gt; GSM49604     4   0.507    0.33668 0.000 0.168  0 0.700 0.132
#&gt; GSM49605     2   0.141    0.94902 0.000 0.940  0 0.000 0.060
#&gt; GSM49606     2   0.141    0.94902 0.000 0.940  0 0.000 0.060
#&gt; GSM49607     2   0.141    0.94902 0.000 0.940  0 0.000 0.060
#&gt; GSM49608     2   0.154    0.94367 0.000 0.932  0 0.000 0.068
#&gt; GSM49609     2   0.000    0.95107 0.000 1.000  0 0.000 0.000
#&gt; GSM49610     2   0.000    0.95107 0.000 1.000  0 0.000 0.000
#&gt; GSM49611     2   0.000    0.95107 0.000 1.000  0 0.000 0.000
#&gt; GSM49612     2   0.000    0.95107 0.000 1.000  0 0.000 0.000
#&gt; GSM49614     3   0.000    1.00000 0.000 0.000  1 0.000 0.000
#&gt; GSM49615     3   0.000    1.00000 0.000 0.000  1 0.000 0.000
#&gt; GSM49616     3   0.000    1.00000 0.000 0.000  1 0.000 0.000
#&gt; GSM49617     3   0.000    1.00000 0.000 0.000  1 0.000 0.000
#&gt; GSM49564     1   0.185    0.90568 0.912 0.000  0 0.000 0.088
#&gt; GSM49565     1   0.167    0.90920 0.924 0.000  0 0.000 0.076
#&gt; GSM49566     1   0.185    0.90568 0.912 0.000  0 0.000 0.088
#&gt; GSM49567     1   0.127    0.91936 0.948 0.000  0 0.000 0.052
#&gt; GSM49568     1   0.161    0.91104 0.928 0.000  0 0.000 0.072
#&gt; GSM49569     1   0.179    0.90737 0.916 0.000  0 0.000 0.084
#&gt; GSM49570     4   0.120    0.54599 0.000 0.000  0 0.952 0.048
#&gt; GSM49571     4   0.578    0.21822 0.332 0.008  0 0.576 0.084
#&gt; GSM49572     1   0.112    0.91927 0.956 0.000  0 0.000 0.044
#&gt; GSM49573     4   0.029    0.55074 0.000 0.000  0 0.992 0.008
#&gt; GSM49574     1   0.173    0.90523 0.920 0.000  0 0.000 0.080
#&gt; GSM49575     1   0.173    0.90523 0.920 0.000  0 0.000 0.080
#&gt; GSM49576     1   0.185    0.90568 0.912 0.000  0 0.000 0.088
#&gt; GSM49577     1   0.173    0.90841 0.920 0.000  0 0.000 0.080
#&gt; GSM49578     1   0.173    0.90523 0.920 0.000  0 0.000 0.080
#&gt; GSM49579     1   0.134    0.91803 0.944 0.000  0 0.000 0.056
#&gt; GSM49580     1   0.120    0.91856 0.952 0.000  0 0.000 0.048
#&gt; GSM49581     1   0.112    0.91895 0.956 0.000  0 0.000 0.044
#&gt; GSM49582     1   0.173    0.90523 0.920 0.000  0 0.000 0.080
#&gt; GSM49583     5   0.367    0.00000 0.000 0.236  0 0.008 0.756
#&gt; GSM49584     1   0.127    0.92126 0.948 0.000  0 0.000 0.052
#&gt; GSM49585     1   0.127    0.92141 0.948 0.000  0 0.000 0.052
#&gt; GSM49586     1   0.596   -0.00109 0.500 0.008  0 0.408 0.084
#&gt; GSM49587     1   0.134    0.91575 0.944 0.000  0 0.000 0.056
#&gt; GSM49588     1   0.134    0.92102 0.944 0.000  0 0.000 0.056
#&gt; GSM49589     1   0.134    0.91672 0.944 0.000  0 0.000 0.056
#&gt; GSM49590     1   0.185    0.90568 0.912 0.000  0 0.000 0.088
#&gt; GSM49591     1   0.167    0.90723 0.924 0.000  0 0.000 0.076
#&gt; GSM49592     1   0.173    0.90523 0.920 0.000  0 0.000 0.080
#&gt; GSM49593     1   0.179    0.90708 0.916 0.000  0 0.000 0.084
#&gt; GSM49594     1   0.179    0.90290 0.916 0.000  0 0.000 0.084
#&gt; GSM49595     1   0.179    0.90290 0.916 0.000  0 0.000 0.084
#&gt; GSM49596     1   0.179    0.90708 0.916 0.000  0 0.000 0.084
#&gt; GSM49597     4   0.402    0.32792 0.000 0.000  0 0.652 0.348
#&gt; GSM49598     1   0.173    0.90898 0.920 0.000  0 0.000 0.080
#&gt; GSM49599     1   0.161    0.91401 0.928 0.000  0 0.000 0.072
#&gt; GSM49600     1   0.112    0.91895 0.956 0.000  0 0.000 0.044
#&gt; GSM49601     1   0.120    0.92210 0.952 0.000  0 0.000 0.048
#&gt; GSM49602     1   0.154    0.91280 0.932 0.000  0 0.000 0.068
#&gt; GSM49603     1   0.154    0.91280 0.932 0.000  0 0.000 0.068
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-4-a').click(function(){
  $('#tab-CV-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-5'>
<p><a id='tab-CV-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2 p3    p4    p5    p6
#&gt; GSM49613     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 0.000
#&gt; GSM49604     5  0.5590     -0.421 0.000 0.184  0 0.196 0.604 0.016
#&gt; GSM49605     2  0.0146      0.830 0.000 0.996  0 0.000 0.004 0.000
#&gt; GSM49606     2  0.0000      0.831 0.000 1.000  0 0.000 0.000 0.000
#&gt; GSM49607     2  0.0146      0.830 0.000 0.996  0 0.000 0.004 0.000
#&gt; GSM49608     2  0.0363      0.824 0.000 0.988  0 0.000 0.012 0.000
#&gt; GSM49609     2  0.3394      0.841 0.000 0.776  0 0.000 0.200 0.024
#&gt; GSM49610     2  0.3394      0.841 0.000 0.776  0 0.000 0.200 0.024
#&gt; GSM49611     2  0.3394      0.841 0.000 0.776  0 0.000 0.200 0.024
#&gt; GSM49612     2  0.3394      0.841 0.000 0.776  0 0.000 0.200 0.024
#&gt; GSM49614     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 0.000
#&gt; GSM49615     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 0.000
#&gt; GSM49616     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 0.000
#&gt; GSM49617     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 0.000
#&gt; GSM49564     1  0.3309      0.700 0.720 0.000  0 0.000 0.000 0.280
#&gt; GSM49565     1  0.2473      0.790 0.856 0.000  0 0.000 0.008 0.136
#&gt; GSM49566     1  0.3221      0.710 0.736 0.000  0 0.000 0.000 0.264
#&gt; GSM49567     1  0.1858      0.805 0.904 0.000  0 0.000 0.004 0.092
#&gt; GSM49568     1  0.2362      0.777 0.860 0.000  0 0.000 0.004 0.136
#&gt; GSM49569     1  0.3023      0.749 0.768 0.000  0 0.000 0.000 0.232
#&gt; GSM49570     4  0.1141      0.564 0.000 0.000  0 0.948 0.052 0.000
#&gt; GSM49571     5  0.6362      0.377 0.236 0.000  0 0.084 0.552 0.128
#&gt; GSM49572     1  0.1910      0.791 0.892 0.000  0 0.000 0.000 0.108
#&gt; GSM49573     4  0.4109      0.419 0.000 0.000  0 0.576 0.412 0.012
#&gt; GSM49574     1  0.2595      0.762 0.836 0.000  0 0.000 0.004 0.160
#&gt; GSM49575     1  0.2980      0.731 0.800 0.000  0 0.000 0.008 0.192
#&gt; GSM49576     1  0.3023      0.739 0.768 0.000  0 0.000 0.000 0.232
#&gt; GSM49577     1  0.2664      0.751 0.816 0.000  0 0.000 0.000 0.184
#&gt; GSM49578     1  0.2558      0.764 0.840 0.000  0 0.000 0.004 0.156
#&gt; GSM49579     1  0.2762      0.780 0.804 0.000  0 0.000 0.000 0.196
#&gt; GSM49580     1  0.0790      0.809 0.968 0.000  0 0.000 0.000 0.032
#&gt; GSM49581     1  0.0790      0.809 0.968 0.000  0 0.000 0.000 0.032
#&gt; GSM49582     1  0.2558      0.764 0.840 0.000  0 0.000 0.004 0.156
#&gt; GSM49583     6  0.6515      0.000 0.000 0.220  0 0.052 0.224 0.504
#&gt; GSM49584     1  0.1556      0.806 0.920 0.000  0 0.000 0.000 0.080
#&gt; GSM49585     1  0.2664      0.804 0.848 0.000  0 0.000 0.016 0.136
#&gt; GSM49586     5  0.6115      0.386 0.376 0.000  0 0.020 0.448 0.156
#&gt; GSM49587     1  0.2178      0.782 0.868 0.000  0 0.000 0.000 0.132
#&gt; GSM49588     1  0.2513      0.807 0.852 0.000  0 0.000 0.008 0.140
#&gt; GSM49589     1  0.2932      0.781 0.820 0.000  0 0.000 0.016 0.164
#&gt; GSM49590     1  0.3050      0.739 0.764 0.000  0 0.000 0.000 0.236
#&gt; GSM49591     1  0.2558      0.765 0.840 0.000  0 0.000 0.004 0.156
#&gt; GSM49592     1  0.2768      0.763 0.832 0.000  0 0.000 0.012 0.156
#&gt; GSM49593     1  0.2854      0.758 0.792 0.000  0 0.000 0.000 0.208
#&gt; GSM49594     1  0.3445      0.778 0.796 0.000  0 0.000 0.048 0.156
#&gt; GSM49595     1  0.3445      0.778 0.796 0.000  0 0.000 0.048 0.156
#&gt; GSM49596     1  0.2854      0.758 0.792 0.000  0 0.000 0.000 0.208
#&gt; GSM49597     4  0.4447      0.335 0.000 0.020  0 0.744 0.092 0.144
#&gt; GSM49598     1  0.2003      0.794 0.884 0.000  0 0.000 0.000 0.116
#&gt; GSM49599     1  0.2632      0.755 0.832 0.000  0 0.000 0.004 0.164
#&gt; GSM49600     1  0.1610      0.805 0.916 0.000  0 0.000 0.000 0.084
#&gt; GSM49601     1  0.2536      0.806 0.864 0.000  0 0.000 0.020 0.116
#&gt; GSM49602     1  0.3133      0.757 0.780 0.000  0 0.000 0.008 0.212
#&gt; GSM49603     1  0.3133      0.757 0.780 0.000  0 0.000 0.008 0.212
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-5-a').click(function(){
  $('#tab-CV-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-CV-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-get-signatures'>
<ul>
<li><a href='#tab-CV-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-1-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-1"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-2-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-2"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-3-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-3"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-4-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-4"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-5-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-hclust-signature_compare](figure_cola/CV-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-CV-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-hclust-collect-classes](figure_cola/CV-hclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n disease.state(p) cell.type(p) k
#> CV:hclust 52         1.08e-06     8.91e-04 2
#> CV:hclust 52         4.99e-07     3.05e-13 3
#> CV:hclust 51         7.28e-08     2.59e-13 4
#> CV:hclust 49         1.78e-09     2.49e-13 5
#> CV:hclust 48         2.78e-09     5.08e-13 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:kmeans






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "kmeans"]
# you can also extract it by
# res = res_list["CV:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21168 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-kmeans-collect-plots](figure_cola/CV-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-kmeans-select-partition-number](figure_cola/CV-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.547           0.958       0.937         0.3373 0.628   0.628
#> 3 3 0.869           0.937       0.946         0.4867 0.874   0.800
#> 4 4 0.723           0.702       0.699         0.2784 0.767   0.538
#> 5 5 0.671           0.894       0.877         0.1461 0.882   0.628
#> 6 6 0.760           0.736       0.849         0.0687 0.988   0.951
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-get-classes'>
<ul>
<li><a href='#tab-CV-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-kmeans-get-classes-1'>
<p><a id='tab-CV-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM49613     1   0.689      0.785 0.816 0.184
#&gt; GSM49604     2   0.689      0.999 0.184 0.816
#&gt; GSM49605     2   0.689      0.999 0.184 0.816
#&gt; GSM49606     2   0.689      0.999 0.184 0.816
#&gt; GSM49607     2   0.689      0.999 0.184 0.816
#&gt; GSM49608     2   0.689      0.999 0.184 0.816
#&gt; GSM49609     2   0.689      0.999 0.184 0.816
#&gt; GSM49610     2   0.689      0.999 0.184 0.816
#&gt; GSM49611     2   0.689      0.999 0.184 0.816
#&gt; GSM49612     2   0.689      0.999 0.184 0.816
#&gt; GSM49614     1   0.689      0.785 0.816 0.184
#&gt; GSM49615     1   0.689      0.785 0.816 0.184
#&gt; GSM49616     1   0.689      0.785 0.816 0.184
#&gt; GSM49617     1   0.689      0.785 0.816 0.184
#&gt; GSM49564     1   0.000      0.969 1.000 0.000
#&gt; GSM49565     1   0.000      0.969 1.000 0.000
#&gt; GSM49566     1   0.000      0.969 1.000 0.000
#&gt; GSM49567     1   0.000      0.969 1.000 0.000
#&gt; GSM49568     1   0.000      0.969 1.000 0.000
#&gt; GSM49569     1   0.000      0.969 1.000 0.000
#&gt; GSM49570     2   0.697      0.996 0.188 0.812
#&gt; GSM49571     1   0.373      0.885 0.928 0.072
#&gt; GSM49572     1   0.000      0.969 1.000 0.000
#&gt; GSM49573     2   0.697      0.996 0.188 0.812
#&gt; GSM49574     1   0.000      0.969 1.000 0.000
#&gt; GSM49575     1   0.000      0.969 1.000 0.000
#&gt; GSM49576     1   0.000      0.969 1.000 0.000
#&gt; GSM49577     1   0.000      0.969 1.000 0.000
#&gt; GSM49578     1   0.000      0.969 1.000 0.000
#&gt; GSM49579     1   0.000      0.969 1.000 0.000
#&gt; GSM49580     1   0.000      0.969 1.000 0.000
#&gt; GSM49581     1   0.000      0.969 1.000 0.000
#&gt; GSM49582     1   0.000      0.969 1.000 0.000
#&gt; GSM49583     2   0.689      0.999 0.184 0.816
#&gt; GSM49584     1   0.000      0.969 1.000 0.000
#&gt; GSM49585     1   0.000      0.969 1.000 0.000
#&gt; GSM49586     1   0.000      0.969 1.000 0.000
#&gt; GSM49587     1   0.000      0.969 1.000 0.000
#&gt; GSM49588     1   0.000      0.969 1.000 0.000
#&gt; GSM49589     1   0.000      0.969 1.000 0.000
#&gt; GSM49590     1   0.000      0.969 1.000 0.000
#&gt; GSM49591     1   0.000      0.969 1.000 0.000
#&gt; GSM49592     1   0.000      0.969 1.000 0.000
#&gt; GSM49593     1   0.000      0.969 1.000 0.000
#&gt; GSM49594     1   0.000      0.969 1.000 0.000
#&gt; GSM49595     1   0.000      0.969 1.000 0.000
#&gt; GSM49596     1   0.000      0.969 1.000 0.000
#&gt; GSM49597     2   0.697      0.996 0.188 0.812
#&gt; GSM49598     1   0.000      0.969 1.000 0.000
#&gt; GSM49599     1   0.000      0.969 1.000 0.000
#&gt; GSM49600     1   0.000      0.969 1.000 0.000
#&gt; GSM49601     1   0.000      0.969 1.000 0.000
#&gt; GSM49602     1   0.000      0.969 1.000 0.000
#&gt; GSM49603     1   0.000      0.969 1.000 0.000
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-1-a').click(function(){
  $('#tab-CV-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-2'>
<p><a id='tab-CV-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM49613     3  0.4178      0.960 0.172 0.000 0.828
#&gt; GSM49604     2  0.3686      0.896 0.000 0.860 0.140
#&gt; GSM49605     2  0.0000      0.949 0.000 1.000 0.000
#&gt; GSM49606     2  0.0424      0.949 0.000 0.992 0.008
#&gt; GSM49607     2  0.0000      0.949 0.000 1.000 0.000
#&gt; GSM49608     2  0.0000      0.949 0.000 1.000 0.000
#&gt; GSM49609     2  0.0892      0.947 0.000 0.980 0.020
#&gt; GSM49610     2  0.0892      0.947 0.000 0.980 0.020
#&gt; GSM49611     2  0.0892      0.947 0.000 0.980 0.020
#&gt; GSM49612     2  0.0892      0.947 0.000 0.980 0.020
#&gt; GSM49614     3  0.2356      0.829 0.072 0.000 0.928
#&gt; GSM49615     3  0.4178      0.960 0.172 0.000 0.828
#&gt; GSM49616     3  0.4178      0.960 0.172 0.000 0.828
#&gt; GSM49617     3  0.4178      0.960 0.172 0.000 0.828
#&gt; GSM49564     1  0.2448      0.941 0.924 0.000 0.076
#&gt; GSM49565     1  0.0000      0.951 1.000 0.000 0.000
#&gt; GSM49566     1  0.2537      0.941 0.920 0.000 0.080
#&gt; GSM49567     1  0.0000      0.951 1.000 0.000 0.000
#&gt; GSM49568     1  0.0000      0.951 1.000 0.000 0.000
#&gt; GSM49569     1  0.2537      0.941 0.920 0.000 0.080
#&gt; GSM49570     2  0.3879      0.890 0.000 0.848 0.152
#&gt; GSM49571     1  0.6004      0.691 0.780 0.064 0.156
#&gt; GSM49572     1  0.0000      0.951 1.000 0.000 0.000
#&gt; GSM49573     2  0.4047      0.890 0.004 0.848 0.148
#&gt; GSM49574     1  0.0000      0.951 1.000 0.000 0.000
#&gt; GSM49575     1  0.0000      0.951 1.000 0.000 0.000
#&gt; GSM49576     1  0.2537      0.941 0.920 0.000 0.080
#&gt; GSM49577     1  0.0237      0.950 0.996 0.000 0.004
#&gt; GSM49578     1  0.0000      0.951 1.000 0.000 0.000
#&gt; GSM49579     1  0.2356      0.944 0.928 0.000 0.072
#&gt; GSM49580     1  0.0892      0.949 0.980 0.000 0.020
#&gt; GSM49581     1  0.0000      0.951 1.000 0.000 0.000
#&gt; GSM49582     1  0.0000      0.951 1.000 0.000 0.000
#&gt; GSM49583     2  0.0237      0.949 0.000 0.996 0.004
#&gt; GSM49584     1  0.0424      0.950 0.992 0.000 0.008
#&gt; GSM49585     1  0.0000      0.951 1.000 0.000 0.000
#&gt; GSM49586     1  0.2066      0.947 0.940 0.000 0.060
#&gt; GSM49587     1  0.0000      0.951 1.000 0.000 0.000
#&gt; GSM49588     1  0.0000      0.951 1.000 0.000 0.000
#&gt; GSM49589     1  0.2537      0.941 0.920 0.000 0.080
#&gt; GSM49590     1  0.2537      0.941 0.920 0.000 0.080
#&gt; GSM49591     1  0.0000      0.951 1.000 0.000 0.000
#&gt; GSM49592     1  0.0000      0.951 1.000 0.000 0.000
#&gt; GSM49593     1  0.2537      0.941 0.920 0.000 0.080
#&gt; GSM49594     1  0.2066      0.947 0.940 0.000 0.060
#&gt; GSM49595     1  0.2066      0.947 0.940 0.000 0.060
#&gt; GSM49596     1  0.2165      0.947 0.936 0.000 0.064
#&gt; GSM49597     2  0.3879      0.890 0.000 0.848 0.152
#&gt; GSM49598     1  0.2537      0.941 0.920 0.000 0.080
#&gt; GSM49599     1  0.0000      0.951 1.000 0.000 0.000
#&gt; GSM49600     1  0.2066      0.947 0.940 0.000 0.060
#&gt; GSM49601     1  0.2066      0.947 0.940 0.000 0.060
#&gt; GSM49602     1  0.2537      0.941 0.920 0.000 0.080
#&gt; GSM49603     1  0.2537      0.941 0.920 0.000 0.080
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-2-a').click(function(){
  $('#tab-CV-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-3'>
<p><a id='tab-CV-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM49613     3  0.6482    0.82756 0.072 0.000 0.504 0.424
#&gt; GSM49604     2  0.4989    0.62057 0.000 0.528 0.472 0.000
#&gt; GSM49605     2  0.1004    0.83496 0.000 0.972 0.024 0.004
#&gt; GSM49606     2  0.0188    0.83190 0.000 0.996 0.000 0.004
#&gt; GSM49607     2  0.1004    0.83496 0.000 0.972 0.024 0.004
#&gt; GSM49608     2  0.1004    0.83496 0.000 0.972 0.024 0.004
#&gt; GSM49609     2  0.1284    0.83224 0.000 0.964 0.024 0.012
#&gt; GSM49610     2  0.1284    0.83224 0.000 0.964 0.024 0.012
#&gt; GSM49611     2  0.1284    0.83224 0.000 0.964 0.024 0.012
#&gt; GSM49612     2  0.1284    0.83224 0.000 0.964 0.024 0.012
#&gt; GSM49614     3  0.6222    0.81273 0.056 0.000 0.532 0.412
#&gt; GSM49615     3  0.6482    0.82756 0.072 0.000 0.504 0.424
#&gt; GSM49616     3  0.6465    0.82787 0.072 0.000 0.516 0.412
#&gt; GSM49617     3  0.6465    0.82787 0.072 0.000 0.516 0.412
#&gt; GSM49564     1  0.1118    0.76959 0.964 0.000 0.000 0.036
#&gt; GSM49565     4  0.4972    0.97379 0.456 0.000 0.000 0.544
#&gt; GSM49566     1  0.1118    0.77671 0.964 0.000 0.000 0.036
#&gt; GSM49567     4  0.4977    0.97049 0.460 0.000 0.000 0.540
#&gt; GSM49568     4  0.4985    0.97758 0.468 0.000 0.000 0.532
#&gt; GSM49569     1  0.0000    0.79412 1.000 0.000 0.000 0.000
#&gt; GSM49570     2  0.5163    0.61283 0.004 0.516 0.480 0.000
#&gt; GSM49571     3  0.7953   -0.00382 0.400 0.080 0.456 0.064
#&gt; GSM49572     4  0.4981    0.97212 0.464 0.000 0.000 0.536
#&gt; GSM49573     2  0.5163    0.61283 0.004 0.516 0.480 0.000
#&gt; GSM49574     4  0.4972    0.97379 0.456 0.000 0.000 0.544
#&gt; GSM49575     4  0.4967    0.96878 0.452 0.000 0.000 0.548
#&gt; GSM49576     1  0.0000    0.79412 1.000 0.000 0.000 0.000
#&gt; GSM49577     1  0.4898   -0.58078 0.584 0.000 0.000 0.416
#&gt; GSM49578     4  0.4985    0.97758 0.468 0.000 0.000 0.532
#&gt; GSM49579     1  0.0469    0.78928 0.988 0.000 0.000 0.012
#&gt; GSM49580     1  0.4999   -0.90033 0.508 0.000 0.000 0.492
#&gt; GSM49581     4  0.4985    0.97758 0.468 0.000 0.000 0.532
#&gt; GSM49582     4  0.4985    0.97758 0.468 0.000 0.000 0.532
#&gt; GSM49583     2  0.1004    0.83496 0.000 0.972 0.024 0.004
#&gt; GSM49584     4  0.5000    0.91434 0.496 0.000 0.000 0.504
#&gt; GSM49585     1  0.4830   -0.52943 0.608 0.000 0.000 0.392
#&gt; GSM49586     1  0.1635    0.76843 0.948 0.000 0.008 0.044
#&gt; GSM49587     4  0.4985    0.97758 0.468 0.000 0.000 0.532
#&gt; GSM49588     1  0.4948   -0.74172 0.560 0.000 0.000 0.440
#&gt; GSM49589     1  0.0524    0.79074 0.988 0.000 0.004 0.008
#&gt; GSM49590     1  0.0188    0.79305 0.996 0.000 0.004 0.000
#&gt; GSM49591     4  0.4985    0.95614 0.468 0.000 0.000 0.532
#&gt; GSM49592     4  0.4977    0.97410 0.460 0.000 0.000 0.540
#&gt; GSM49593     1  0.0000    0.79412 1.000 0.000 0.000 0.000
#&gt; GSM49594     1  0.1661    0.76739 0.944 0.000 0.004 0.052
#&gt; GSM49595     1  0.1661    0.76739 0.944 0.000 0.004 0.052
#&gt; GSM49596     1  0.0592    0.78776 0.984 0.000 0.000 0.016
#&gt; GSM49597     2  0.5308    0.61636 0.004 0.540 0.452 0.004
#&gt; GSM49598     1  0.0000    0.79412 1.000 0.000 0.000 0.000
#&gt; GSM49599     4  0.4972    0.97167 0.456 0.000 0.000 0.544
#&gt; GSM49600     1  0.3444    0.42887 0.816 0.000 0.000 0.184
#&gt; GSM49601     1  0.0592    0.79106 0.984 0.000 0.000 0.016
#&gt; GSM49602     1  0.0376    0.79298 0.992 0.000 0.004 0.004
#&gt; GSM49603     1  0.0376    0.79298 0.992 0.000 0.004 0.004
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-3-a').click(function(){
  $('#tab-CV-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-4'>
<p><a id='tab-CV-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM49613     3  0.0324      0.997 0.004 0.000 0.992 0.000 0.004
#&gt; GSM49604     4  0.2648      0.897 0.000 0.152 0.000 0.848 0.000
#&gt; GSM49605     2  0.1668      0.914 0.028 0.940 0.000 0.032 0.000
#&gt; GSM49606     2  0.0510      0.920 0.000 0.984 0.000 0.016 0.000
#&gt; GSM49607     2  0.1915      0.909 0.040 0.928 0.000 0.032 0.000
#&gt; GSM49608     2  0.1668      0.914 0.028 0.940 0.000 0.032 0.000
#&gt; GSM49609     2  0.2074      0.913 0.060 0.920 0.004 0.016 0.000
#&gt; GSM49610     2  0.2074      0.913 0.060 0.920 0.004 0.016 0.000
#&gt; GSM49611     2  0.2074      0.913 0.060 0.920 0.004 0.016 0.000
#&gt; GSM49612     2  0.2074      0.913 0.060 0.920 0.004 0.016 0.000
#&gt; GSM49614     3  0.0451      0.993 0.008 0.000 0.988 0.000 0.004
#&gt; GSM49615     3  0.0324      0.997 0.004 0.000 0.992 0.000 0.004
#&gt; GSM49616     3  0.0162      0.997 0.000 0.000 0.996 0.000 0.004
#&gt; GSM49617     3  0.0162      0.997 0.000 0.000 0.996 0.000 0.004
#&gt; GSM49564     5  0.2291      0.895 0.036 0.000 0.000 0.056 0.908
#&gt; GSM49565     1  0.3695      0.908 0.800 0.000 0.000 0.036 0.164
#&gt; GSM49566     5  0.2300      0.881 0.024 0.000 0.000 0.072 0.904
#&gt; GSM49567     1  0.3922      0.901 0.780 0.000 0.000 0.040 0.180
#&gt; GSM49568     1  0.3381      0.917 0.808 0.000 0.000 0.016 0.176
#&gt; GSM49569     5  0.0451      0.927 0.008 0.000 0.000 0.004 0.988
#&gt; GSM49570     4  0.2877      0.898 0.004 0.144 0.004 0.848 0.000
#&gt; GSM49571     4  0.3817      0.691 0.056 0.012 0.000 0.824 0.108
#&gt; GSM49572     1  0.3883      0.903 0.780 0.000 0.000 0.036 0.184
#&gt; GSM49573     4  0.2719      0.898 0.004 0.144 0.000 0.852 0.000
#&gt; GSM49574     1  0.3183      0.913 0.828 0.000 0.000 0.016 0.156
#&gt; GSM49575     1  0.2920      0.903 0.852 0.000 0.000 0.016 0.132
#&gt; GSM49576     5  0.0693      0.922 0.012 0.000 0.000 0.008 0.980
#&gt; GSM49577     1  0.5533      0.609 0.580 0.000 0.000 0.084 0.336
#&gt; GSM49578     1  0.3171      0.917 0.816 0.000 0.000 0.008 0.176
#&gt; GSM49579     5  0.0807      0.920 0.012 0.000 0.000 0.012 0.976
#&gt; GSM49580     1  0.3940      0.888 0.756 0.000 0.000 0.024 0.220
#&gt; GSM49581     1  0.3419      0.915 0.804 0.000 0.000 0.016 0.180
#&gt; GSM49582     1  0.3132      0.917 0.820 0.000 0.000 0.008 0.172
#&gt; GSM49583     2  0.2580      0.881 0.044 0.892 0.000 0.064 0.000
#&gt; GSM49584     1  0.3745      0.905 0.780 0.000 0.000 0.024 0.196
#&gt; GSM49585     1  0.5063      0.711 0.632 0.000 0.000 0.056 0.312
#&gt; GSM49586     5  0.2992      0.891 0.068 0.000 0.000 0.064 0.868
#&gt; GSM49587     1  0.3132      0.918 0.820 0.000 0.000 0.008 0.172
#&gt; GSM49588     1  0.4452      0.800 0.696 0.000 0.000 0.032 0.272
#&gt; GSM49589     5  0.1915      0.921 0.032 0.000 0.000 0.040 0.928
#&gt; GSM49590     5  0.0290      0.926 0.000 0.000 0.000 0.008 0.992
#&gt; GSM49591     1  0.3734      0.904 0.796 0.000 0.000 0.036 0.168
#&gt; GSM49592     1  0.3695      0.910 0.800 0.000 0.000 0.036 0.164
#&gt; GSM49593     5  0.0671      0.926 0.016 0.000 0.000 0.004 0.980
#&gt; GSM49594     5  0.2580      0.899 0.044 0.000 0.000 0.064 0.892
#&gt; GSM49595     5  0.2580      0.899 0.044 0.000 0.000 0.064 0.892
#&gt; GSM49596     5  0.0671      0.926 0.016 0.000 0.000 0.004 0.980
#&gt; GSM49597     4  0.4150      0.851 0.044 0.180 0.004 0.772 0.000
#&gt; GSM49598     5  0.0609      0.926 0.020 0.000 0.000 0.000 0.980
#&gt; GSM49599     1  0.3276      0.893 0.836 0.000 0.000 0.032 0.132
#&gt; GSM49600     5  0.4065      0.504 0.264 0.000 0.000 0.016 0.720
#&gt; GSM49601     5  0.2370      0.908 0.040 0.000 0.000 0.056 0.904
#&gt; GSM49602     5  0.1579      0.924 0.032 0.000 0.000 0.024 0.944
#&gt; GSM49603     5  0.1579      0.924 0.032 0.000 0.000 0.024 0.944
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-4-a').click(function(){
  $('#tab-CV-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-5'>
<p><a id='tab-CV-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM49613     3  0.0862      0.976 0.008 0.000 0.972 0.004 0.000 0.016
#&gt; GSM49604     4  0.2011      0.887 0.004 0.064 0.000 0.912 0.000 0.020
#&gt; GSM49605     2  0.3374      0.864 0.000 0.772 0.000 0.020 0.000 0.208
#&gt; GSM49606     2  0.2703      0.871 0.000 0.824 0.000 0.004 0.000 0.172
#&gt; GSM49607     2  0.3711      0.840 0.000 0.720 0.000 0.020 0.000 0.260
#&gt; GSM49608     2  0.3374      0.864 0.000 0.772 0.000 0.020 0.000 0.208
#&gt; GSM49609     2  0.0146      0.861 0.000 0.996 0.000 0.004 0.000 0.000
#&gt; GSM49610     2  0.0146      0.861 0.000 0.996 0.000 0.004 0.000 0.000
#&gt; GSM49611     2  0.0146      0.861 0.000 0.996 0.000 0.004 0.000 0.000
#&gt; GSM49612     2  0.0146      0.861 0.000 0.996 0.000 0.004 0.000 0.000
#&gt; GSM49614     3  0.1500      0.949 0.012 0.000 0.936 0.000 0.000 0.052
#&gt; GSM49615     3  0.0862      0.976 0.008 0.000 0.972 0.004 0.000 0.016
#&gt; GSM49616     3  0.0000      0.977 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49617     3  0.0000      0.977 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49564     5  0.4148      0.643 0.048 0.000 0.000 0.016 0.748 0.188
#&gt; GSM49565     1  0.3892      0.604 0.752 0.000 0.000 0.000 0.060 0.188
#&gt; GSM49566     5  0.4086      0.538 0.028 0.000 0.000 0.008 0.708 0.256
#&gt; GSM49567     1  0.4400      0.410 0.684 0.000 0.000 0.000 0.068 0.248
#&gt; GSM49568     1  0.2231      0.713 0.900 0.000 0.000 0.004 0.068 0.028
#&gt; GSM49569     5  0.0291      0.844 0.004 0.000 0.000 0.000 0.992 0.004
#&gt; GSM49570     4  0.1194      0.896 0.004 0.032 0.000 0.956 0.000 0.008
#&gt; GSM49571     4  0.3839      0.750 0.012 0.004 0.000 0.776 0.032 0.176
#&gt; GSM49572     1  0.4394      0.548 0.716 0.000 0.000 0.008 0.068 0.208
#&gt; GSM49573     4  0.0790      0.896 0.000 0.032 0.000 0.968 0.000 0.000
#&gt; GSM49574     1  0.3521      0.685 0.812 0.000 0.000 0.008 0.060 0.120
#&gt; GSM49575     1  0.3771      0.542 0.764 0.000 0.000 0.000 0.056 0.180
#&gt; GSM49576     5  0.0405      0.842 0.008 0.000 0.000 0.000 0.988 0.004
#&gt; GSM49577     6  0.6057      0.000 0.360 0.000 0.000 0.004 0.216 0.420
#&gt; GSM49578     1  0.1787      0.718 0.920 0.000 0.000 0.004 0.068 0.008
#&gt; GSM49579     5  0.1218      0.838 0.012 0.000 0.000 0.004 0.956 0.028
#&gt; GSM49580     1  0.4100      0.574 0.760 0.000 0.000 0.004 0.112 0.124
#&gt; GSM49581     1  0.3047      0.689 0.848 0.000 0.000 0.004 0.084 0.064
#&gt; GSM49582     1  0.1787      0.718 0.920 0.000 0.000 0.004 0.068 0.008
#&gt; GSM49583     2  0.4711      0.800 0.020 0.676 0.000 0.052 0.000 0.252
#&gt; GSM49584     1  0.3565      0.644 0.808 0.000 0.000 0.004 0.096 0.092
#&gt; GSM49585     1  0.5510      0.262 0.604 0.000 0.000 0.012 0.220 0.164
#&gt; GSM49586     5  0.4181      0.675 0.028 0.000 0.000 0.012 0.704 0.256
#&gt; GSM49587     1  0.2361      0.718 0.896 0.000 0.000 0.008 0.064 0.032
#&gt; GSM49588     1  0.4733      0.513 0.704 0.000 0.000 0.012 0.172 0.112
#&gt; GSM49589     5  0.2784      0.819 0.012 0.000 0.000 0.008 0.848 0.132
#&gt; GSM49590     5  0.0520      0.844 0.008 0.000 0.000 0.000 0.984 0.008
#&gt; GSM49591     1  0.3844      0.671 0.796 0.000 0.000 0.016 0.076 0.112
#&gt; GSM49592     1  0.2962      0.697 0.848 0.000 0.000 0.000 0.068 0.084
#&gt; GSM49593     5  0.0405      0.845 0.008 0.000 0.000 0.000 0.988 0.004
#&gt; GSM49594     5  0.3486      0.756 0.024 0.000 0.000 0.008 0.788 0.180
#&gt; GSM49595     5  0.3486      0.756 0.024 0.000 0.000 0.008 0.788 0.180
#&gt; GSM49596     5  0.0405      0.845 0.008 0.000 0.000 0.000 0.988 0.004
#&gt; GSM49597     4  0.3629      0.828 0.024 0.032 0.000 0.804 0.000 0.140
#&gt; GSM49598     5  0.0291      0.844 0.004 0.000 0.000 0.000 0.992 0.004
#&gt; GSM49599     1  0.4739      0.322 0.636 0.000 0.000 0.008 0.056 0.300
#&gt; GSM49600     5  0.4663      0.346 0.244 0.000 0.000 0.004 0.672 0.080
#&gt; GSM49601     5  0.3000      0.791 0.016 0.000 0.000 0.004 0.824 0.156
#&gt; GSM49602     5  0.1644      0.837 0.004 0.000 0.000 0.000 0.920 0.076
#&gt; GSM49603     5  0.1644      0.837 0.004 0.000 0.000 0.000 0.920 0.076
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-5-a').click(function(){
  $('#tab-CV-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-CV-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-get-signatures'>
<ul>
<li><a href='#tab-CV-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-kmeans-signature_compare](figure_cola/CV-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-CV-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-kmeans-collect-classes](figure_cola/CV-kmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n disease.state(p) cell.type(p) k
#> CV:kmeans 54         5.97e-07     6.72e-04 2
#> CV:kmeans 54         2.57e-07     8.46e-14 3
#> CV:kmeans 48         8.02e-06     6.07e-11 4
#> CV:kmeans 54         1.42e-07     4.23e-13 5
#> CV:kmeans 49         8.68e-07     1.20e-11 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:skmeans






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "skmeans"]
# you can also extract it by
# res = res_list["CV:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21168 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-skmeans-collect-plots](figure_cola/CV-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-skmeans-select-partition-number](figure_cola/CV-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.742           0.923       0.963         0.4712 0.525   0.525
#> 3 3 0.672           0.814       0.905         0.4244 0.709   0.490
#> 4 4 0.624           0.733       0.841         0.1212 0.878   0.652
#> 5 5 0.605           0.547       0.741         0.0616 0.966   0.870
#> 6 6 0.640           0.427       0.677         0.0400 0.967   0.856
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-get-classes'>
<ul>
<li><a href='#tab-CV-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-skmeans-get-classes-1'>
<p><a id='tab-CV-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM49613     1  0.0000      0.973 1.000 0.000
#&gt; GSM49604     2  0.0000      0.935 0.000 1.000
#&gt; GSM49605     2  0.0000      0.935 0.000 1.000
#&gt; GSM49606     2  0.0000      0.935 0.000 1.000
#&gt; GSM49607     2  0.0000      0.935 0.000 1.000
#&gt; GSM49608     2  0.0000      0.935 0.000 1.000
#&gt; GSM49609     2  0.0000      0.935 0.000 1.000
#&gt; GSM49610     2  0.0000      0.935 0.000 1.000
#&gt; GSM49611     2  0.0000      0.935 0.000 1.000
#&gt; GSM49612     2  0.0000      0.935 0.000 1.000
#&gt; GSM49614     2  0.7950      0.712 0.240 0.760
#&gt; GSM49615     1  0.0000      0.973 1.000 0.000
#&gt; GSM49616     1  0.0672      0.967 0.992 0.008
#&gt; GSM49617     1  0.6623      0.785 0.828 0.172
#&gt; GSM49564     1  0.0000      0.973 1.000 0.000
#&gt; GSM49565     1  0.6801      0.782 0.820 0.180
#&gt; GSM49566     1  0.0000      0.973 1.000 0.000
#&gt; GSM49567     1  0.5408      0.851 0.876 0.124
#&gt; GSM49568     1  0.0000      0.973 1.000 0.000
#&gt; GSM49569     1  0.0000      0.973 1.000 0.000
#&gt; GSM49570     2  0.0000      0.935 0.000 1.000
#&gt; GSM49571     2  0.0000      0.935 0.000 1.000
#&gt; GSM49572     1  0.0376      0.970 0.996 0.004
#&gt; GSM49573     2  0.0000      0.935 0.000 1.000
#&gt; GSM49574     1  0.5408      0.855 0.876 0.124
#&gt; GSM49575     2  0.9087      0.567 0.324 0.676
#&gt; GSM49576     1  0.0000      0.973 1.000 0.000
#&gt; GSM49577     2  0.2603      0.910 0.044 0.956
#&gt; GSM49578     1  0.0000      0.973 1.000 0.000
#&gt; GSM49579     1  0.0000      0.973 1.000 0.000
#&gt; GSM49580     1  0.0000      0.973 1.000 0.000
#&gt; GSM49581     1  0.0000      0.973 1.000 0.000
#&gt; GSM49582     1  0.0000      0.973 1.000 0.000
#&gt; GSM49583     2  0.0000      0.935 0.000 1.000
#&gt; GSM49584     1  0.0000      0.973 1.000 0.000
#&gt; GSM49585     1  0.0000      0.973 1.000 0.000
#&gt; GSM49586     2  0.6531      0.805 0.168 0.832
#&gt; GSM49587     1  0.0000      0.973 1.000 0.000
#&gt; GSM49588     1  0.0000      0.973 1.000 0.000
#&gt; GSM49589     1  0.0000      0.973 1.000 0.000
#&gt; GSM49590     1  0.0000      0.973 1.000 0.000
#&gt; GSM49591     1  0.0000      0.973 1.000 0.000
#&gt; GSM49592     1  0.0000      0.973 1.000 0.000
#&gt; GSM49593     1  0.0000      0.973 1.000 0.000
#&gt; GSM49594     2  0.6623      0.800 0.172 0.828
#&gt; GSM49595     1  0.7219      0.749 0.800 0.200
#&gt; GSM49596     1  0.0000      0.973 1.000 0.000
#&gt; GSM49597     2  0.0000      0.935 0.000 1.000
#&gt; GSM49598     1  0.0000      0.973 1.000 0.000
#&gt; GSM49599     2  0.8016      0.712 0.244 0.756
#&gt; GSM49600     1  0.0000      0.973 1.000 0.000
#&gt; GSM49601     1  0.0000      0.973 1.000 0.000
#&gt; GSM49602     1  0.0000      0.973 1.000 0.000
#&gt; GSM49603     1  0.0000      0.973 1.000 0.000
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-1-a').click(function(){
  $('#tab-CV-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-2'>
<p><a id='tab-CV-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM49613     3  0.0000     0.8538 0.000 0.000 1.000
#&gt; GSM49604     2  0.0000     0.9344 0.000 1.000 0.000
#&gt; GSM49605     2  0.0000     0.9344 0.000 1.000 0.000
#&gt; GSM49606     2  0.0000     0.9344 0.000 1.000 0.000
#&gt; GSM49607     2  0.0000     0.9344 0.000 1.000 0.000
#&gt; GSM49608     2  0.0000     0.9344 0.000 1.000 0.000
#&gt; GSM49609     2  0.0000     0.9344 0.000 1.000 0.000
#&gt; GSM49610     2  0.0000     0.9344 0.000 1.000 0.000
#&gt; GSM49611     2  0.0000     0.9344 0.000 1.000 0.000
#&gt; GSM49612     2  0.0000     0.9344 0.000 1.000 0.000
#&gt; GSM49614     3  0.3752     0.7469 0.000 0.144 0.856
#&gt; GSM49615     3  0.0000     0.8538 0.000 0.000 1.000
#&gt; GSM49616     3  0.0000     0.8538 0.000 0.000 1.000
#&gt; GSM49617     3  0.0000     0.8538 0.000 0.000 1.000
#&gt; GSM49564     3  0.1163     0.8548 0.028 0.000 0.972
#&gt; GSM49565     1  0.0000     0.9071 1.000 0.000 0.000
#&gt; GSM49566     3  0.3192     0.8293 0.112 0.000 0.888
#&gt; GSM49567     1  0.1781     0.8967 0.960 0.020 0.020
#&gt; GSM49568     1  0.0747     0.9062 0.984 0.000 0.016
#&gt; GSM49569     3  0.2066     0.8568 0.060 0.000 0.940
#&gt; GSM49570     2  0.0000     0.9344 0.000 1.000 0.000
#&gt; GSM49571     2  0.0237     0.9312 0.000 0.996 0.004
#&gt; GSM49572     1  0.0829     0.9070 0.984 0.004 0.012
#&gt; GSM49573     2  0.0000     0.9344 0.000 1.000 0.000
#&gt; GSM49574     1  0.0237     0.9071 0.996 0.004 0.000
#&gt; GSM49575     1  0.2537     0.8628 0.920 0.080 0.000
#&gt; GSM49576     3  0.0892     0.8575 0.020 0.000 0.980
#&gt; GSM49577     1  0.7801     0.1903 0.520 0.428 0.052
#&gt; GSM49578     1  0.0424     0.9069 0.992 0.000 0.008
#&gt; GSM49579     3  0.5431     0.7041 0.284 0.000 0.716
#&gt; GSM49580     1  0.4121     0.7557 0.832 0.000 0.168
#&gt; GSM49581     1  0.0747     0.9057 0.984 0.000 0.016
#&gt; GSM49582     1  0.0000     0.9071 1.000 0.000 0.000
#&gt; GSM49583     2  0.0000     0.9344 0.000 1.000 0.000
#&gt; GSM49584     1  0.1964     0.8860 0.944 0.000 0.056
#&gt; GSM49585     1  0.3340     0.8200 0.880 0.000 0.120
#&gt; GSM49586     2  0.8637     0.3282 0.128 0.564 0.308
#&gt; GSM49587     1  0.0424     0.9072 0.992 0.000 0.008
#&gt; GSM49588     1  0.2625     0.8552 0.916 0.000 0.084
#&gt; GSM49589     3  0.0592     0.8567 0.012 0.000 0.988
#&gt; GSM49590     3  0.0237     0.8541 0.004 0.000 0.996
#&gt; GSM49591     1  0.0000     0.9071 1.000 0.000 0.000
#&gt; GSM49592     1  0.0000     0.9071 1.000 0.000 0.000
#&gt; GSM49593     3  0.4291     0.8101 0.180 0.000 0.820
#&gt; GSM49594     2  0.9679     0.0545 0.232 0.448 0.320
#&gt; GSM49595     3  0.8597     0.4184 0.380 0.104 0.516
#&gt; GSM49596     3  0.6126     0.5304 0.400 0.000 0.600
#&gt; GSM49597     2  0.0000     0.9344 0.000 1.000 0.000
#&gt; GSM49598     3  0.5058     0.7581 0.244 0.000 0.756
#&gt; GSM49599     1  0.6487     0.6110 0.700 0.268 0.032
#&gt; GSM49600     3  0.6026     0.5464 0.376 0.000 0.624
#&gt; GSM49601     3  0.4605     0.7929 0.204 0.000 0.796
#&gt; GSM49602     3  0.3116     0.8450 0.108 0.000 0.892
#&gt; GSM49603     3  0.2959     0.8471 0.100 0.000 0.900
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-2-a').click(function(){
  $('#tab-CV-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-3'>
<p><a id='tab-CV-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM49613     3  0.0000     0.8727 0.000 0.000 1.000 0.000
#&gt; GSM49604     2  0.0188     0.9798 0.000 0.996 0.000 0.004
#&gt; GSM49605     2  0.0000     0.9816 0.000 1.000 0.000 0.000
#&gt; GSM49606     2  0.0000     0.9816 0.000 1.000 0.000 0.000
#&gt; GSM49607     2  0.0000     0.9816 0.000 1.000 0.000 0.000
#&gt; GSM49608     2  0.0000     0.9816 0.000 1.000 0.000 0.000
#&gt; GSM49609     2  0.0000     0.9816 0.000 1.000 0.000 0.000
#&gt; GSM49610     2  0.0000     0.9816 0.000 1.000 0.000 0.000
#&gt; GSM49611     2  0.0000     0.9816 0.000 1.000 0.000 0.000
#&gt; GSM49612     2  0.0000     0.9816 0.000 1.000 0.000 0.000
#&gt; GSM49614     3  0.0779     0.8576 0.000 0.016 0.980 0.004
#&gt; GSM49615     3  0.0000     0.8727 0.000 0.000 1.000 0.000
#&gt; GSM49616     3  0.0000     0.8727 0.000 0.000 1.000 0.000
#&gt; GSM49617     3  0.0000     0.8727 0.000 0.000 1.000 0.000
#&gt; GSM49564     3  0.2797     0.8161 0.032 0.000 0.900 0.068
#&gt; GSM49565     1  0.2760     0.7687 0.872 0.000 0.000 0.128
#&gt; GSM49566     4  0.7843     0.1801 0.172 0.012 0.404 0.412
#&gt; GSM49567     1  0.3093     0.7688 0.884 0.004 0.020 0.092
#&gt; GSM49568     1  0.2973     0.7742 0.884 0.000 0.020 0.096
#&gt; GSM49569     4  0.6079     0.4554 0.052 0.000 0.380 0.568
#&gt; GSM49570     2  0.0804     0.9718 0.000 0.980 0.008 0.012
#&gt; GSM49571     2  0.4588     0.7567 0.012 0.788 0.024 0.176
#&gt; GSM49572     1  0.2053     0.7732 0.924 0.000 0.004 0.072
#&gt; GSM49573     2  0.0469     0.9759 0.000 0.988 0.000 0.012
#&gt; GSM49574     1  0.3052     0.7641 0.860 0.004 0.000 0.136
#&gt; GSM49575     1  0.5460     0.6998 0.736 0.040 0.020 0.204
#&gt; GSM49576     4  0.5510     0.2124 0.016 0.000 0.480 0.504
#&gt; GSM49577     1  0.8789     0.1388 0.388 0.264 0.044 0.304
#&gt; GSM49578     1  0.0469     0.7680 0.988 0.000 0.000 0.012
#&gt; GSM49579     4  0.6363     0.6360 0.172 0.000 0.172 0.656
#&gt; GSM49580     1  0.6159     0.5823 0.672 0.000 0.196 0.132
#&gt; GSM49581     1  0.3052     0.7580 0.860 0.000 0.004 0.136
#&gt; GSM49582     1  0.0592     0.7683 0.984 0.000 0.000 0.016
#&gt; GSM49583     2  0.0000     0.9816 0.000 1.000 0.000 0.000
#&gt; GSM49584     1  0.3697     0.7377 0.852 0.000 0.100 0.048
#&gt; GSM49585     1  0.6613     0.4287 0.560 0.000 0.096 0.344
#&gt; GSM49586     4  0.4191     0.7100 0.024 0.068 0.060 0.848
#&gt; GSM49587     1  0.1807     0.7739 0.940 0.000 0.008 0.052
#&gt; GSM49588     1  0.5512     0.5761 0.660 0.000 0.040 0.300
#&gt; GSM49589     3  0.3945     0.6515 0.004 0.000 0.780 0.216
#&gt; GSM49590     3  0.4679     0.2652 0.000 0.000 0.648 0.352
#&gt; GSM49591     1  0.4008     0.7004 0.756 0.000 0.000 0.244
#&gt; GSM49592     1  0.2011     0.7727 0.920 0.000 0.000 0.080
#&gt; GSM49593     4  0.4332     0.7482 0.072 0.000 0.112 0.816
#&gt; GSM49594     4  0.4604     0.6869 0.040 0.100 0.036 0.824
#&gt; GSM49595     4  0.2335     0.7363 0.060 0.000 0.020 0.920
#&gt; GSM49596     4  0.5218     0.6664 0.200 0.000 0.064 0.736
#&gt; GSM49597     2  0.0376     0.9779 0.000 0.992 0.004 0.004
#&gt; GSM49598     4  0.4953     0.7412 0.104 0.000 0.120 0.776
#&gt; GSM49599     1  0.8024     0.5161 0.584 0.168 0.076 0.172
#&gt; GSM49600     1  0.7543    -0.0729 0.420 0.000 0.188 0.392
#&gt; GSM49601     4  0.3474     0.7477 0.064 0.000 0.068 0.868
#&gt; GSM49602     4  0.3392     0.7426 0.020 0.000 0.124 0.856
#&gt; GSM49603     4  0.3577     0.7318 0.012 0.000 0.156 0.832
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-3-a').click(function(){
  $('#tab-CV-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-4'>
<p><a id='tab-CV-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM49613     3  0.0000     0.8018 0.000 0.000 1.000 0.000 0.000
#&gt; GSM49604     2  0.2338     0.8640 0.000 0.884 0.000 0.112 0.004
#&gt; GSM49605     2  0.0000     0.9060 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49606     2  0.0324     0.9058 0.000 0.992 0.000 0.004 0.004
#&gt; GSM49607     2  0.0324     0.9057 0.000 0.992 0.000 0.004 0.004
#&gt; GSM49608     2  0.0451     0.9057 0.000 0.988 0.000 0.008 0.004
#&gt; GSM49609     2  0.0451     0.9051 0.000 0.988 0.000 0.008 0.004
#&gt; GSM49610     2  0.0162     0.9061 0.000 0.996 0.000 0.004 0.000
#&gt; GSM49611     2  0.0451     0.9051 0.000 0.988 0.000 0.008 0.004
#&gt; GSM49612     2  0.0451     0.9051 0.000 0.988 0.000 0.008 0.004
#&gt; GSM49614     3  0.0865     0.7859 0.000 0.004 0.972 0.024 0.000
#&gt; GSM49615     3  0.0000     0.8018 0.000 0.000 1.000 0.000 0.000
#&gt; GSM49616     3  0.0000     0.8018 0.000 0.000 1.000 0.000 0.000
#&gt; GSM49617     3  0.0290     0.7992 0.000 0.000 0.992 0.008 0.000
#&gt; GSM49564     3  0.5042     0.6370 0.064 0.000 0.760 0.080 0.096
#&gt; GSM49565     1  0.5137     0.4888 0.676 0.004 0.000 0.244 0.076
#&gt; GSM49566     5  0.8367    -0.0223 0.140 0.000 0.252 0.304 0.304
#&gt; GSM49567     1  0.5661     0.3501 0.596 0.004 0.012 0.332 0.056
#&gt; GSM49568     1  0.4512     0.5500 0.776 0.000 0.016 0.132 0.076
#&gt; GSM49569     5  0.7155     0.3736 0.056 0.000 0.272 0.160 0.512
#&gt; GSM49570     2  0.4032     0.7867 0.000 0.772 0.032 0.192 0.004
#&gt; GSM49571     2  0.6663     0.4143 0.016 0.548 0.012 0.296 0.128
#&gt; GSM49572     1  0.5112     0.4655 0.664 0.000 0.004 0.268 0.064
#&gt; GSM49573     2  0.3643     0.7880 0.000 0.776 0.004 0.212 0.008
#&gt; GSM49574     1  0.4901     0.5267 0.716 0.000 0.000 0.168 0.116
#&gt; GSM49575     1  0.6431     0.1991 0.560 0.044 0.012 0.332 0.052
#&gt; GSM49576     5  0.7072     0.2892 0.028 0.000 0.340 0.184 0.448
#&gt; GSM49577     4  0.8692     0.2237 0.228 0.192 0.020 0.392 0.168
#&gt; GSM49578     1  0.1469     0.5858 0.948 0.000 0.000 0.036 0.016
#&gt; GSM49579     5  0.6861     0.2706 0.092 0.000 0.064 0.332 0.512
#&gt; GSM49580     1  0.7308     0.1693 0.508 0.000 0.104 0.280 0.108
#&gt; GSM49581     1  0.5371     0.4470 0.684 0.000 0.012 0.208 0.096
#&gt; GSM49582     1  0.1787     0.5812 0.936 0.000 0.004 0.044 0.016
#&gt; GSM49583     2  0.1121     0.8960 0.000 0.956 0.000 0.044 0.000
#&gt; GSM49584     1  0.5515     0.4703 0.680 0.000 0.056 0.224 0.040
#&gt; GSM49585     1  0.7669     0.0972 0.424 0.000 0.064 0.228 0.284
#&gt; GSM49586     5  0.5717     0.4029 0.024 0.060 0.004 0.272 0.640
#&gt; GSM49587     1  0.3764     0.5814 0.800 0.000 0.000 0.156 0.044
#&gt; GSM49588     1  0.6540     0.3464 0.544 0.000 0.012 0.220 0.224
#&gt; GSM49589     3  0.5948     0.3138 0.012 0.000 0.572 0.092 0.324
#&gt; GSM49590     3  0.6144     0.0235 0.024 0.000 0.512 0.072 0.392
#&gt; GSM49591     1  0.5367     0.4830 0.668 0.000 0.000 0.184 0.148
#&gt; GSM49592     1  0.3471     0.5697 0.836 0.000 0.000 0.092 0.072
#&gt; GSM49593     5  0.5478     0.5587 0.080 0.000 0.084 0.108 0.728
#&gt; GSM49594     5  0.6645     0.4144 0.036 0.100 0.036 0.188 0.640
#&gt; GSM49595     5  0.4009     0.5440 0.036 0.004 0.008 0.152 0.800
#&gt; GSM49596     5  0.6546     0.4582 0.172 0.000 0.052 0.164 0.612
#&gt; GSM49597     2  0.2773     0.8580 0.000 0.868 0.020 0.112 0.000
#&gt; GSM49598     5  0.6077     0.5266 0.084 0.000 0.072 0.180 0.664
#&gt; GSM49599     4  0.8030     0.0478 0.336 0.100 0.048 0.444 0.072
#&gt; GSM49600     4  0.8551     0.0211 0.272 0.000 0.192 0.280 0.256
#&gt; GSM49601     5  0.4393     0.5395 0.088 0.000 0.020 0.100 0.792
#&gt; GSM49602     5  0.2772     0.5893 0.028 0.000 0.044 0.032 0.896
#&gt; GSM49603     5  0.4180     0.5852 0.040 0.000 0.080 0.064 0.816
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-4-a').click(function(){
  $('#tab-CV-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-5'>
<p><a id='tab-CV-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM49613     3  0.0260     0.7747 0.000 0.000 0.992 0.000 0.008 0.000
#&gt; GSM49604     2  0.2994     0.5798 0.000 0.788 0.000 0.208 0.000 0.004
#&gt; GSM49605     2  0.0508     0.8005 0.000 0.984 0.000 0.012 0.000 0.004
#&gt; GSM49606     2  0.0458     0.8003 0.000 0.984 0.000 0.016 0.000 0.000
#&gt; GSM49607     2  0.0363     0.7997 0.000 0.988 0.000 0.012 0.000 0.000
#&gt; GSM49608     2  0.0547     0.8010 0.000 0.980 0.000 0.020 0.000 0.000
#&gt; GSM49609     2  0.1082     0.7881 0.000 0.956 0.000 0.040 0.000 0.004
#&gt; GSM49610     2  0.0935     0.7980 0.000 0.964 0.000 0.032 0.000 0.004
#&gt; GSM49611     2  0.0935     0.7917 0.000 0.964 0.000 0.032 0.000 0.004
#&gt; GSM49612     2  0.0935     0.7923 0.000 0.964 0.000 0.032 0.000 0.004
#&gt; GSM49614     3  0.0547     0.7674 0.000 0.000 0.980 0.020 0.000 0.000
#&gt; GSM49615     3  0.0146     0.7752 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; GSM49616     3  0.0291     0.7754 0.000 0.000 0.992 0.004 0.004 0.000
#&gt; GSM49617     3  0.0146     0.7741 0.000 0.000 0.996 0.004 0.000 0.000
#&gt; GSM49564     3  0.6215     0.5106 0.072 0.000 0.656 0.068 0.112 0.092
#&gt; GSM49565     1  0.5698     0.4263 0.580 0.012 0.000 0.060 0.036 0.312
#&gt; GSM49566     5  0.8700    -0.0496 0.124 0.004 0.144 0.152 0.324 0.252
#&gt; GSM49567     1  0.6369     0.2344 0.448 0.004 0.012 0.116 0.024 0.396
#&gt; GSM49568     1  0.4857     0.4941 0.732 0.000 0.016 0.052 0.040 0.160
#&gt; GSM49569     5  0.7201     0.3343 0.036 0.000 0.148 0.124 0.532 0.160
#&gt; GSM49570     2  0.4640     0.0933 0.000 0.604 0.044 0.348 0.000 0.004
#&gt; GSM49571     4  0.6470     0.0000 0.012 0.368 0.016 0.480 0.100 0.024
#&gt; GSM49572     1  0.5511     0.4279 0.568 0.000 0.008 0.068 0.020 0.336
#&gt; GSM49573     2  0.3727     0.0709 0.000 0.612 0.000 0.388 0.000 0.000
#&gt; GSM49574     1  0.5453     0.4662 0.604 0.004 0.000 0.056 0.040 0.296
#&gt; GSM49575     1  0.7182     0.2487 0.500 0.024 0.012 0.192 0.048 0.224
#&gt; GSM49576     5  0.7716     0.1957 0.020 0.000 0.272 0.196 0.384 0.128
#&gt; GSM49577     6  0.8779     0.1463 0.164 0.164 0.012 0.224 0.096 0.340
#&gt; GSM49578     1  0.2458     0.5419 0.892 0.000 0.000 0.024 0.016 0.068
#&gt; GSM49579     6  0.7993    -0.1528 0.060 0.004 0.072 0.196 0.328 0.340
#&gt; GSM49580     1  0.7375     0.2025 0.464 0.000 0.064 0.072 0.108 0.292
#&gt; GSM49581     1  0.6399     0.3035 0.544 0.000 0.000 0.084 0.128 0.244
#&gt; GSM49582     1  0.2136     0.5440 0.908 0.000 0.000 0.016 0.012 0.064
#&gt; GSM49583     2  0.1788     0.7621 0.000 0.916 0.004 0.076 0.000 0.004
#&gt; GSM49584     1  0.5805     0.4234 0.620 0.000 0.044 0.040 0.040 0.256
#&gt; GSM49585     6  0.7892    -0.0767 0.308 0.000 0.036 0.108 0.204 0.344
#&gt; GSM49586     5  0.6054     0.3045 0.008 0.052 0.004 0.308 0.560 0.068
#&gt; GSM49587     1  0.3999     0.5188 0.744 0.000 0.000 0.020 0.024 0.212
#&gt; GSM49588     1  0.6938     0.2306 0.496 0.000 0.020 0.056 0.184 0.244
#&gt; GSM49589     3  0.6931     0.1916 0.020 0.000 0.492 0.096 0.292 0.100
#&gt; GSM49590     3  0.6674     0.0203 0.004 0.000 0.452 0.112 0.352 0.080
#&gt; GSM49591     1  0.5912     0.3658 0.556 0.000 0.000 0.060 0.080 0.304
#&gt; GSM49592     1  0.4686     0.4825 0.712 0.000 0.000 0.020 0.084 0.184
#&gt; GSM49593     5  0.4405     0.4981 0.020 0.000 0.032 0.080 0.784 0.084
#&gt; GSM49594     5  0.7585     0.1994 0.028 0.084 0.008 0.220 0.460 0.200
#&gt; GSM49595     5  0.5928     0.3706 0.032 0.000 0.004 0.200 0.596 0.168
#&gt; GSM49596     5  0.6709     0.3273 0.128 0.000 0.020 0.104 0.572 0.176
#&gt; GSM49597     2  0.3678     0.5335 0.000 0.752 0.024 0.220 0.000 0.004
#&gt; GSM49598     5  0.5221     0.4497 0.036 0.000 0.012 0.140 0.704 0.108
#&gt; GSM49599     6  0.8635     0.0997 0.188 0.104 0.024 0.300 0.068 0.316
#&gt; GSM49600     6  0.8547     0.0469 0.240 0.000 0.092 0.128 0.256 0.284
#&gt; GSM49601     5  0.5029     0.4340 0.060 0.000 0.008 0.084 0.728 0.120
#&gt; GSM49602     5  0.2816     0.5112 0.004 0.000 0.012 0.064 0.876 0.044
#&gt; GSM49603     5  0.2911     0.5160 0.012 0.000 0.032 0.048 0.880 0.028
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-5-a').click(function(){
  $('#tab-CV-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-CV-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-get-signatures'>
<ul>
<li><a href='#tab-CV-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-skmeans-signature_compare](figure_cola/CV-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-CV-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-skmeans-collect-classes](figure_cola/CV-skmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n disease.state(p) cell.type(p) k
#> CV:skmeans 54         5.02e-04     2.14e-02 2
#> CV:skmeans 50         6.18e-06     6.36e-04 3
#> CV:skmeans 47         4.68e-05     2.11e-07 4
#> CV:skmeans 31         2.28e-03     3.45e-05 5
#> CV:skmeans 22         7.25e-03     2.15e-03 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:pam**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "pam"]
# you can also extract it by
# res = res_list["CV:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21168 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-pam-collect-plots](figure_cola/CV-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-pam-select-partition-number](figure_cola/CV-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.962       0.984         0.3228 0.669   0.669
#> 3 3 0.566           0.839       0.885         0.5597 0.867   0.802
#> 4 4 0.590           0.741       0.863         0.3689 0.727   0.504
#> 5 5 0.623           0.726       0.857         0.0656 0.917   0.736
#> 6 6 0.669           0.727       0.881         0.0296 0.983   0.934
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-pam-get-classes'>
<ul>
<li><a href='#tab-CV-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-pam-get-classes-1'>
<p><a id='tab-CV-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM49613     1  0.0376      0.990 0.996 0.004
#&gt; GSM49604     2  0.4022      0.892 0.080 0.920
#&gt; GSM49605     2  0.0376      0.943 0.004 0.996
#&gt; GSM49606     2  0.0376      0.943 0.004 0.996
#&gt; GSM49607     2  0.0376      0.943 0.004 0.996
#&gt; GSM49608     2  0.0376      0.943 0.004 0.996
#&gt; GSM49609     2  0.0376      0.943 0.004 0.996
#&gt; GSM49610     2  0.0376      0.943 0.004 0.996
#&gt; GSM49611     2  0.0376      0.943 0.004 0.996
#&gt; GSM49612     2  0.0376      0.943 0.004 0.996
#&gt; GSM49614     1  0.0376      0.990 0.996 0.004
#&gt; GSM49615     1  0.0376      0.990 0.996 0.004
#&gt; GSM49616     1  0.0376      0.990 0.996 0.004
#&gt; GSM49617     1  0.0376      0.990 0.996 0.004
#&gt; GSM49564     1  0.0000      0.993 1.000 0.000
#&gt; GSM49565     1  0.0000      0.993 1.000 0.000
#&gt; GSM49566     1  0.0000      0.993 1.000 0.000
#&gt; GSM49567     1  0.0000      0.993 1.000 0.000
#&gt; GSM49568     1  0.0000      0.993 1.000 0.000
#&gt; GSM49569     1  0.0000      0.993 1.000 0.000
#&gt; GSM49570     1  0.7950      0.655 0.760 0.240
#&gt; GSM49571     1  0.0000      0.993 1.000 0.000
#&gt; GSM49572     1  0.0000      0.993 1.000 0.000
#&gt; GSM49573     1  0.0376      0.990 0.996 0.004
#&gt; GSM49574     1  0.0000      0.993 1.000 0.000
#&gt; GSM49575     1  0.0000      0.993 1.000 0.000
#&gt; GSM49576     1  0.0000      0.993 1.000 0.000
#&gt; GSM49577     1  0.0000      0.993 1.000 0.000
#&gt; GSM49578     1  0.0000      0.993 1.000 0.000
#&gt; GSM49579     1  0.0000      0.993 1.000 0.000
#&gt; GSM49580     1  0.0000      0.993 1.000 0.000
#&gt; GSM49581     1  0.0000      0.993 1.000 0.000
#&gt; GSM49582     1  0.0000      0.993 1.000 0.000
#&gt; GSM49583     2  0.3431      0.906 0.064 0.936
#&gt; GSM49584     1  0.0000      0.993 1.000 0.000
#&gt; GSM49585     1  0.0000      0.993 1.000 0.000
#&gt; GSM49586     1  0.0000      0.993 1.000 0.000
#&gt; GSM49587     1  0.0000      0.993 1.000 0.000
#&gt; GSM49588     1  0.0000      0.993 1.000 0.000
#&gt; GSM49589     1  0.0000      0.993 1.000 0.000
#&gt; GSM49590     1  0.0000      0.993 1.000 0.000
#&gt; GSM49591     1  0.0000      0.993 1.000 0.000
#&gt; GSM49592     1  0.0000      0.993 1.000 0.000
#&gt; GSM49593     1  0.0000      0.993 1.000 0.000
#&gt; GSM49594     1  0.0000      0.993 1.000 0.000
#&gt; GSM49595     1  0.0000      0.993 1.000 0.000
#&gt; GSM49596     1  0.0000      0.993 1.000 0.000
#&gt; GSM49597     2  0.9850      0.274 0.428 0.572
#&gt; GSM49598     1  0.0000      0.993 1.000 0.000
#&gt; GSM49599     1  0.0000      0.993 1.000 0.000
#&gt; GSM49600     1  0.0000      0.993 1.000 0.000
#&gt; GSM49601     1  0.0000      0.993 1.000 0.000
#&gt; GSM49602     1  0.0000      0.993 1.000 0.000
#&gt; GSM49603     1  0.0000      0.993 1.000 0.000
</code></pre>

<script>
$('#tab-CV-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-1-a').click(function(){
  $('#tab-CV-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-2'>
<p><a id='tab-CV-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM49613     3  0.1411   0.896194 0.036 0.000 0.964
#&gt; GSM49604     2  0.3856   0.780821 0.072 0.888 0.040
#&gt; GSM49605     2  0.0000   0.893978 0.000 1.000 0.000
#&gt; GSM49606     2  0.0000   0.893978 0.000 1.000 0.000
#&gt; GSM49607     2  0.0000   0.893978 0.000 1.000 0.000
#&gt; GSM49608     2  0.0000   0.893978 0.000 1.000 0.000
#&gt; GSM49609     2  0.0000   0.893978 0.000 1.000 0.000
#&gt; GSM49610     2  0.0000   0.893978 0.000 1.000 0.000
#&gt; GSM49611     2  0.0000   0.893978 0.000 1.000 0.000
#&gt; GSM49612     2  0.0000   0.893978 0.000 1.000 0.000
#&gt; GSM49614     3  0.2066   0.893608 0.060 0.000 0.940
#&gt; GSM49615     3  0.3752   0.863300 0.144 0.000 0.856
#&gt; GSM49616     3  0.2066   0.906452 0.060 0.000 0.940
#&gt; GSM49617     3  0.3686   0.876580 0.140 0.000 0.860
#&gt; GSM49564     1  0.5098   0.862066 0.752 0.000 0.248
#&gt; GSM49565     1  0.1163   0.824817 0.972 0.000 0.028
#&gt; GSM49566     1  0.4931   0.865190 0.768 0.000 0.232
#&gt; GSM49567     1  0.0892   0.821073 0.980 0.000 0.020
#&gt; GSM49568     1  0.0892   0.821073 0.980 0.000 0.020
#&gt; GSM49569     1  0.5138   0.860823 0.748 0.000 0.252
#&gt; GSM49570     1  0.8894   0.599574 0.572 0.192 0.236
#&gt; GSM49571     1  0.4605   0.869388 0.796 0.000 0.204
#&gt; GSM49572     1  0.0747   0.824010 0.984 0.000 0.016
#&gt; GSM49573     1  0.5247   0.864704 0.768 0.008 0.224
#&gt; GSM49574     1  0.2066   0.842190 0.940 0.000 0.060
#&gt; GSM49575     1  0.2625   0.847582 0.916 0.000 0.084
#&gt; GSM49576     1  0.4178   0.871476 0.828 0.000 0.172
#&gt; GSM49577     1  0.4002   0.851071 0.840 0.000 0.160
#&gt; GSM49578     1  0.1289   0.823868 0.968 0.000 0.032
#&gt; GSM49579     1  0.2959   0.860369 0.900 0.000 0.100
#&gt; GSM49580     1  0.3412   0.868587 0.876 0.000 0.124
#&gt; GSM49581     1  0.1964   0.843192 0.944 0.000 0.056
#&gt; GSM49582     1  0.0892   0.821073 0.980 0.000 0.020
#&gt; GSM49583     2  0.2773   0.835098 0.048 0.928 0.024
#&gt; GSM49584     1  0.2356   0.848616 0.928 0.000 0.072
#&gt; GSM49585     1  0.3340   0.857137 0.880 0.000 0.120
#&gt; GSM49586     1  0.5138   0.860823 0.748 0.000 0.252
#&gt; GSM49587     1  0.0892   0.821073 0.980 0.000 0.020
#&gt; GSM49588     1  0.2959   0.855113 0.900 0.000 0.100
#&gt; GSM49589     1  0.5138   0.860823 0.748 0.000 0.252
#&gt; GSM49590     1  0.5098   0.861701 0.752 0.000 0.248
#&gt; GSM49591     1  0.0592   0.826582 0.988 0.000 0.012
#&gt; GSM49592     1  0.1289   0.824092 0.968 0.000 0.032
#&gt; GSM49593     1  0.5138   0.860823 0.748 0.000 0.252
#&gt; GSM49594     1  0.4887   0.868772 0.772 0.000 0.228
#&gt; GSM49595     1  0.5138   0.860823 0.748 0.000 0.252
#&gt; GSM49596     1  0.4974   0.866270 0.764 0.000 0.236
#&gt; GSM49597     2  0.9335  -0.000253 0.324 0.492 0.184
#&gt; GSM49598     1  0.4654   0.869575 0.792 0.000 0.208
#&gt; GSM49599     1  0.4062   0.871813 0.836 0.000 0.164
#&gt; GSM49600     1  0.4842   0.866578 0.776 0.000 0.224
#&gt; GSM49601     1  0.4654   0.866620 0.792 0.000 0.208
#&gt; GSM49602     1  0.5138   0.860823 0.748 0.000 0.252
#&gt; GSM49603     1  0.5138   0.860823 0.748 0.000 0.252
</code></pre>

<script>
$('#tab-CV-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-2-a').click(function(){
  $('#tab-CV-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-3'>
<p><a id='tab-CV-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2 p3    p4
#&gt; GSM49613     3  0.0000     1.0000 0.000 0.000  1 0.000
#&gt; GSM49604     2  0.3009     0.8667 0.052 0.892  0 0.056
#&gt; GSM49605     2  0.0188     0.9713 0.000 0.996  0 0.004
#&gt; GSM49606     2  0.0000     0.9742 0.000 1.000  0 0.000
#&gt; GSM49607     2  0.0000     0.9742 0.000 1.000  0 0.000
#&gt; GSM49608     2  0.0000     0.9742 0.000 1.000  0 0.000
#&gt; GSM49609     2  0.0000     0.9742 0.000 1.000  0 0.000
#&gt; GSM49610     2  0.0000     0.9742 0.000 1.000  0 0.000
#&gt; GSM49611     2  0.0000     0.9742 0.000 1.000  0 0.000
#&gt; GSM49612     2  0.0000     0.9742 0.000 1.000  0 0.000
#&gt; GSM49614     3  0.0000     1.0000 0.000 0.000  1 0.000
#&gt; GSM49615     3  0.0000     1.0000 0.000 0.000  1 0.000
#&gt; GSM49616     3  0.0000     1.0000 0.000 0.000  1 0.000
#&gt; GSM49617     3  0.0000     1.0000 0.000 0.000  1 0.000
#&gt; GSM49564     4  0.2530     0.8290 0.112 0.000  0 0.888
#&gt; GSM49565     1  0.1389     0.7477 0.952 0.000  0 0.048
#&gt; GSM49566     4  0.3311     0.8045 0.172 0.000  0 0.828
#&gt; GSM49567     1  0.0000     0.7439 1.000 0.000  0 0.000
#&gt; GSM49568     1  0.0336     0.7464 0.992 0.000  0 0.008
#&gt; GSM49569     4  0.1716     0.8343 0.064 0.000  0 0.936
#&gt; GSM49570     4  0.3266     0.7585 0.084 0.040  0 0.876
#&gt; GSM49571     1  0.4998     0.2196 0.512 0.000  0 0.488
#&gt; GSM49572     1  0.0469     0.7478 0.988 0.000  0 0.012
#&gt; GSM49573     4  0.3123     0.7498 0.156 0.000  0 0.844
#&gt; GSM49574     1  0.4999    -0.2018 0.508 0.000  0 0.492
#&gt; GSM49575     1  0.4543     0.4099 0.676 0.000  0 0.324
#&gt; GSM49576     4  0.4454     0.6231 0.308 0.000  0 0.692
#&gt; GSM49577     4  0.4431     0.6233 0.304 0.000  0 0.696
#&gt; GSM49578     1  0.1302     0.7431 0.956 0.000  0 0.044
#&gt; GSM49579     1  0.4040     0.6252 0.752 0.000  0 0.248
#&gt; GSM49580     1  0.4989    -0.0525 0.528 0.000  0 0.472
#&gt; GSM49581     1  0.3942     0.5580 0.764 0.000  0 0.236
#&gt; GSM49582     1  0.0000     0.7439 1.000 0.000  0 0.000
#&gt; GSM49583     2  0.2282     0.9093 0.024 0.924  0 0.052
#&gt; GSM49584     1  0.2408     0.7240 0.896 0.000  0 0.104
#&gt; GSM49585     1  0.4356     0.5931 0.708 0.000  0 0.292
#&gt; GSM49586     4  0.1716     0.8343 0.064 0.000  0 0.936
#&gt; GSM49587     1  0.0188     0.7453 0.996 0.000  0 0.004
#&gt; GSM49588     1  0.3649     0.6628 0.796 0.000  0 0.204
#&gt; GSM49589     4  0.2868     0.8066 0.136 0.000  0 0.864
#&gt; GSM49590     4  0.2530     0.8299 0.112 0.000  0 0.888
#&gt; GSM49591     1  0.1637     0.7485 0.940 0.000  0 0.060
#&gt; GSM49592     1  0.2281     0.7238 0.904 0.000  0 0.096
#&gt; GSM49593     4  0.1716     0.8343 0.064 0.000  0 0.936
#&gt; GSM49594     4  0.2530     0.8268 0.112 0.000  0 0.888
#&gt; GSM49595     4  0.1867     0.8358 0.072 0.000  0 0.928
#&gt; GSM49596     4  0.3486     0.7445 0.188 0.000  0 0.812
#&gt; GSM49597     4  0.4319     0.5899 0.012 0.228  0 0.760
#&gt; GSM49598     4  0.4746     0.3866 0.368 0.000  0 0.632
#&gt; GSM49599     4  0.4454     0.6352 0.308 0.000  0 0.692
#&gt; GSM49600     4  0.3837     0.7667 0.224 0.000  0 0.776
#&gt; GSM49601     1  0.4996     0.2248 0.516 0.000  0 0.484
#&gt; GSM49602     4  0.1716     0.8343 0.064 0.000  0 0.936
#&gt; GSM49603     4  0.1792     0.8343 0.068 0.000  0 0.932
</code></pre>

<script>
$('#tab-CV-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-3-a').click(function(){
  $('#tab-CV-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-4'>
<p><a id='tab-CV-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2 p3    p4    p5
#&gt; GSM49613     3  0.0000      1.000 0.000 0.000  1 0.000 0.000
#&gt; GSM49604     2  0.3942      0.850 0.020 0.748  0 0.232 0.000
#&gt; GSM49605     2  0.0290      0.884 0.000 0.992  0 0.008 0.000
#&gt; GSM49606     2  0.0290      0.887 0.000 0.992  0 0.008 0.000
#&gt; GSM49607     2  0.0162      0.885 0.000 0.996  0 0.004 0.000
#&gt; GSM49608     2  0.0290      0.884 0.000 0.992  0 0.008 0.000
#&gt; GSM49609     2  0.3210      0.881 0.000 0.788  0 0.212 0.000
#&gt; GSM49610     2  0.3210      0.881 0.000 0.788  0 0.212 0.000
#&gt; GSM49611     2  0.3210      0.881 0.000 0.788  0 0.212 0.000
#&gt; GSM49612     2  0.3210      0.881 0.000 0.788  0 0.212 0.000
#&gt; GSM49614     3  0.0000      1.000 0.000 0.000  1 0.000 0.000
#&gt; GSM49615     3  0.0000      1.000 0.000 0.000  1 0.000 0.000
#&gt; GSM49616     3  0.0000      1.000 0.000 0.000  1 0.000 0.000
#&gt; GSM49617     3  0.0000      1.000 0.000 0.000  1 0.000 0.000
#&gt; GSM49564     5  0.0880      0.736 0.032 0.000  0 0.000 0.968
#&gt; GSM49565     1  0.1043      0.779 0.960 0.000  0 0.000 0.040
#&gt; GSM49566     5  0.3305      0.634 0.224 0.000  0 0.000 0.776
#&gt; GSM49567     1  0.0000      0.775 1.000 0.000  0 0.000 0.000
#&gt; GSM49568     1  0.0162      0.776 0.996 0.000  0 0.000 0.004
#&gt; GSM49569     5  0.0000      0.736 0.000 0.000  0 0.000 1.000
#&gt; GSM49570     4  0.3760      0.921 0.028 0.000  0 0.784 0.188
#&gt; GSM49571     5  0.3966      0.392 0.336 0.000  0 0.000 0.664
#&gt; GSM49572     1  0.0404      0.779 0.988 0.000  0 0.000 0.012
#&gt; GSM49573     4  0.3885      0.913 0.040 0.000  0 0.784 0.176
#&gt; GSM49574     1  0.4138      0.271 0.616 0.000  0 0.000 0.384
#&gt; GSM49575     1  0.3684      0.534 0.720 0.000  0 0.000 0.280
#&gt; GSM49576     5  0.4045      0.446 0.356 0.000  0 0.000 0.644
#&gt; GSM49577     5  0.3857      0.522 0.312 0.000  0 0.000 0.688
#&gt; GSM49578     1  0.1608      0.757 0.928 0.000  0 0.000 0.072
#&gt; GSM49579     1  0.3366      0.641 0.768 0.000  0 0.000 0.232
#&gt; GSM49580     1  0.4219      0.227 0.584 0.000  0 0.000 0.416
#&gt; GSM49581     1  0.3074      0.658 0.804 0.000  0 0.000 0.196
#&gt; GSM49582     1  0.0000      0.775 1.000 0.000  0 0.000 0.000
#&gt; GSM49583     2  0.1121      0.866 0.000 0.956  0 0.044 0.000
#&gt; GSM49584     1  0.2020      0.755 0.900 0.000  0 0.000 0.100
#&gt; GSM49585     1  0.4182      0.348 0.600 0.000  0 0.000 0.400
#&gt; GSM49586     5  0.0000      0.736 0.000 0.000  0 0.000 1.000
#&gt; GSM49587     1  0.0000      0.775 1.000 0.000  0 0.000 0.000
#&gt; GSM49588     1  0.3109      0.672 0.800 0.000  0 0.000 0.200
#&gt; GSM49589     5  0.1270      0.735 0.052 0.000  0 0.000 0.948
#&gt; GSM49590     5  0.2471      0.692 0.136 0.000  0 0.000 0.864
#&gt; GSM49591     1  0.1410      0.781 0.940 0.000  0 0.000 0.060
#&gt; GSM49592     1  0.3003      0.658 0.812 0.000  0 0.000 0.188
#&gt; GSM49593     5  0.0000      0.736 0.000 0.000  0 0.000 1.000
#&gt; GSM49594     5  0.1671      0.725 0.076 0.000  0 0.000 0.924
#&gt; GSM49595     5  0.0290      0.738 0.008 0.000  0 0.000 0.992
#&gt; GSM49596     5  0.1792      0.721 0.084 0.000  0 0.000 0.916
#&gt; GSM49597     4  0.4194      0.853 0.004 0.012  0 0.708 0.276
#&gt; GSM49598     5  0.3707      0.579 0.284 0.000  0 0.000 0.716
#&gt; GSM49599     5  0.4219      0.266 0.416 0.000  0 0.000 0.584
#&gt; GSM49600     5  0.3837      0.539 0.308 0.000  0 0.000 0.692
#&gt; GSM49601     5  0.3983      0.380 0.340 0.000  0 0.000 0.660
#&gt; GSM49602     5  0.0000      0.736 0.000 0.000  0 0.000 1.000
#&gt; GSM49603     5  0.0000      0.736 0.000 0.000  0 0.000 1.000
</code></pre>

<script>
$('#tab-CV-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-4-a').click(function(){
  $('#tab-CV-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-5'>
<p><a id='tab-CV-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2 p3    p4    p5    p6
#&gt; GSM49613     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 0.000
#&gt; GSM49604     2  0.1536      0.858 0.016 0.940  0 0.004 0.000 0.040
#&gt; GSM49605     6  0.0260      0.953 0.000 0.008  0 0.000 0.000 0.992
#&gt; GSM49606     2  0.3817      0.209 0.000 0.568  0 0.000 0.000 0.432
#&gt; GSM49607     6  0.0260      0.953 0.000 0.008  0 0.000 0.000 0.992
#&gt; GSM49608     6  0.0713      0.947 0.000 0.028  0 0.000 0.000 0.972
#&gt; GSM49609     2  0.0000      0.890 0.000 1.000  0 0.000 0.000 0.000
#&gt; GSM49610     2  0.0000      0.890 0.000 1.000  0 0.000 0.000 0.000
#&gt; GSM49611     2  0.0000      0.890 0.000 1.000  0 0.000 0.000 0.000
#&gt; GSM49612     2  0.0000      0.890 0.000 1.000  0 0.000 0.000 0.000
#&gt; GSM49614     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 0.000
#&gt; GSM49615     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 0.000
#&gt; GSM49616     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 0.000
#&gt; GSM49617     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 0.000
#&gt; GSM49564     5  0.0713      0.785 0.028 0.000  0 0.000 0.972 0.000
#&gt; GSM49565     1  0.0547      0.794 0.980 0.000  0 0.000 0.020 0.000
#&gt; GSM49566     5  0.3189      0.633 0.236 0.000  0 0.000 0.760 0.004
#&gt; GSM49567     1  0.0146      0.792 0.996 0.000  0 0.000 0.000 0.004
#&gt; GSM49568     1  0.0146      0.793 0.996 0.000  0 0.000 0.004 0.000
#&gt; GSM49569     5  0.0000      0.786 0.000 0.000  0 0.000 1.000 0.000
#&gt; GSM49570     4  0.0000      0.834 0.000 0.000  0 1.000 0.000 0.000
#&gt; GSM49571     5  0.3446      0.452 0.308 0.000  0 0.000 0.692 0.000
#&gt; GSM49572     1  0.0603      0.795 0.980 0.000  0 0.000 0.016 0.004
#&gt; GSM49573     4  0.0000      0.834 0.000 0.000  0 1.000 0.000 0.000
#&gt; GSM49574     1  0.3620      0.358 0.648 0.000  0 0.000 0.352 0.000
#&gt; GSM49575     1  0.3221      0.567 0.736 0.000  0 0.000 0.264 0.000
#&gt; GSM49576     5  0.3782      0.428 0.360 0.000  0 0.000 0.636 0.004
#&gt; GSM49577     5  0.3636      0.514 0.320 0.000  0 0.000 0.676 0.004
#&gt; GSM49578     1  0.1556      0.765 0.920 0.000  0 0.000 0.080 0.000
#&gt; GSM49579     1  0.3081      0.662 0.776 0.000  0 0.000 0.220 0.004
#&gt; GSM49580     1  0.3756      0.293 0.600 0.000  0 0.000 0.400 0.000
#&gt; GSM49581     1  0.2597      0.689 0.824 0.000  0 0.000 0.176 0.000
#&gt; GSM49582     1  0.0000      0.791 1.000 0.000  0 0.000 0.000 0.000
#&gt; GSM49583     6  0.1663      0.889 0.000 0.088  0 0.000 0.000 0.912
#&gt; GSM49584     1  0.1814      0.766 0.900 0.000  0 0.000 0.100 0.000
#&gt; GSM49585     1  0.3774      0.334 0.592 0.000  0 0.000 0.408 0.000
#&gt; GSM49586     5  0.0000      0.786 0.000 0.000  0 0.000 1.000 0.000
#&gt; GSM49587     1  0.0000      0.791 1.000 0.000  0 0.000 0.000 0.000
#&gt; GSM49588     1  0.2730      0.690 0.808 0.000  0 0.000 0.192 0.000
#&gt; GSM49589     5  0.1141      0.780 0.052 0.000  0 0.000 0.948 0.000
#&gt; GSM49590     5  0.2482      0.717 0.148 0.000  0 0.000 0.848 0.004
#&gt; GSM49591     1  0.1267      0.791 0.940 0.000  0 0.000 0.060 0.000
#&gt; GSM49592     1  0.2823      0.643 0.796 0.000  0 0.000 0.204 0.000
#&gt; GSM49593     5  0.0000      0.786 0.000 0.000  0 0.000 1.000 0.000
#&gt; GSM49594     5  0.1610      0.761 0.084 0.000  0 0.000 0.916 0.000
#&gt; GSM49595     5  0.0260      0.787 0.008 0.000  0 0.000 0.992 0.000
#&gt; GSM49596     5  0.1327      0.772 0.064 0.000  0 0.000 0.936 0.000
#&gt; GSM49597     4  0.4423      0.573 0.000 0.000  0 0.668 0.060 0.272
#&gt; GSM49598     5  0.3290      0.631 0.252 0.000  0 0.000 0.744 0.004
#&gt; GSM49599     5  0.3971      0.167 0.448 0.000  0 0.000 0.548 0.004
#&gt; GSM49600     5  0.3565      0.542 0.304 0.000  0 0.000 0.692 0.004
#&gt; GSM49601     5  0.3482      0.431 0.316 0.000  0 0.000 0.684 0.000
#&gt; GSM49602     5  0.0000      0.786 0.000 0.000  0 0.000 1.000 0.000
#&gt; GSM49603     5  0.0000      0.786 0.000 0.000  0 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-CV-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-5-a').click(function(){
  $('#tab-CV-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-pam-membership-heatmap'>
<ul>
<li><a href='#tab-CV-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-pam-get-signatures'>
<ul>
<li><a href='#tab-CV-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-1-1.png" alt="plot of chunk tab-CV-pam-get-signatures-1"/></p>

</div>
<div id='tab-CV-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-2-1.png" alt="plot of chunk tab-CV-pam-get-signatures-2"/></p>

</div>
<div id='tab-CV-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-3-1.png" alt="plot of chunk tab-CV-pam-get-signatures-3"/></p>

</div>
<div id='tab-CV-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-4-1.png" alt="plot of chunk tab-CV-pam-get-signatures-4"/></p>

</div>
<div id='tab-CV-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-5-1.png" alt="plot of chunk tab-CV-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-pam-signature_compare](figure_cola/CV-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-pam-dimension-reduction'>
<ul>
<li><a href='#tab-CV-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-pam-collect-classes](figure_cola/CV-pam-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>         n disease.state(p) cell.type(p) k
#> CV:pam 53         2.95e-09     6.95e-05 2
#> CV:pam 53         7.99e-10     1.20e-14 3
#> CV:pam 48         3.28e-08     6.15e-12 4
#> CV:pam 47         1.93e-07     1.29e-10 5
#> CV:pam 46         8.86e-07     2.22e-13 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:mclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "mclust"]
# you can also extract it by
# res = res_list["CV:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21168 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-mclust-collect-plots](figure_cola/CV-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-mclust-select-partition-number](figure_cola/CV-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.826           0.930       0.967         0.4376 0.547   0.547
#> 3 3 0.957           0.947       0.971         0.1634 0.955   0.917
#> 4 4 0.643           0.805       0.844         0.1934 0.979   0.958
#> 5 5 0.619           0.620       0.841         0.1768 0.783   0.550
#> 6 6 0.637           0.446       0.731         0.0952 0.892   0.647
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-get-classes'>
<ul>
<li><a href='#tab-CV-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-mclust-get-classes-1'>
<p><a id='tab-CV-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM49613     2  0.8327      0.719 0.264 0.736
#&gt; GSM49604     2  0.0000      0.918 0.000 1.000
#&gt; GSM49605     2  0.0000      0.918 0.000 1.000
#&gt; GSM49606     2  0.0000      0.918 0.000 1.000
#&gt; GSM49607     2  0.0000      0.918 0.000 1.000
#&gt; GSM49608     2  0.0000      0.918 0.000 1.000
#&gt; GSM49609     2  0.0000      0.918 0.000 1.000
#&gt; GSM49610     2  0.0000      0.918 0.000 1.000
#&gt; GSM49611     2  0.0000      0.918 0.000 1.000
#&gt; GSM49612     2  0.0000      0.918 0.000 1.000
#&gt; GSM49614     2  0.8267      0.724 0.260 0.740
#&gt; GSM49615     2  0.8327      0.719 0.264 0.736
#&gt; GSM49616     2  0.8327      0.719 0.264 0.736
#&gt; GSM49617     2  0.8327      0.719 0.264 0.736
#&gt; GSM49564     1  0.0000      0.986 1.000 0.000
#&gt; GSM49565     1  0.0000      0.986 1.000 0.000
#&gt; GSM49566     1  0.0000      0.986 1.000 0.000
#&gt; GSM49567     1  0.0000      0.986 1.000 0.000
#&gt; GSM49568     1  0.0000      0.986 1.000 0.000
#&gt; GSM49569     1  0.0000      0.986 1.000 0.000
#&gt; GSM49570     2  0.0000      0.918 0.000 1.000
#&gt; GSM49571     1  0.9686      0.243 0.604 0.396
#&gt; GSM49572     1  0.0000      0.986 1.000 0.000
#&gt; GSM49573     2  0.0672      0.913 0.008 0.992
#&gt; GSM49574     1  0.0000      0.986 1.000 0.000
#&gt; GSM49575     1  0.0000      0.986 1.000 0.000
#&gt; GSM49576     1  0.0000      0.986 1.000 0.000
#&gt; GSM49577     1  0.0000      0.986 1.000 0.000
#&gt; GSM49578     1  0.0000      0.986 1.000 0.000
#&gt; GSM49579     1  0.0000      0.986 1.000 0.000
#&gt; GSM49580     1  0.0000      0.986 1.000 0.000
#&gt; GSM49581     1  0.0000      0.986 1.000 0.000
#&gt; GSM49582     1  0.0000      0.986 1.000 0.000
#&gt; GSM49583     2  0.0000      0.918 0.000 1.000
#&gt; GSM49584     1  0.0000      0.986 1.000 0.000
#&gt; GSM49585     1  0.0000      0.986 1.000 0.000
#&gt; GSM49586     1  0.0000      0.986 1.000 0.000
#&gt; GSM49587     1  0.0000      0.986 1.000 0.000
#&gt; GSM49588     1  0.0000      0.986 1.000 0.000
#&gt; GSM49589     1  0.0000      0.986 1.000 0.000
#&gt; GSM49590     1  0.0376      0.982 0.996 0.004
#&gt; GSM49591     1  0.0000      0.986 1.000 0.000
#&gt; GSM49592     1  0.0000      0.986 1.000 0.000
#&gt; GSM49593     1  0.0000      0.986 1.000 0.000
#&gt; GSM49594     1  0.0000      0.986 1.000 0.000
#&gt; GSM49595     1  0.0000      0.986 1.000 0.000
#&gt; GSM49596     1  0.0000      0.986 1.000 0.000
#&gt; GSM49597     2  0.0000      0.918 0.000 1.000
#&gt; GSM49598     1  0.0000      0.986 1.000 0.000
#&gt; GSM49599     1  0.2423      0.943 0.960 0.040
#&gt; GSM49600     1  0.0000      0.986 1.000 0.000
#&gt; GSM49601     1  0.0000      0.986 1.000 0.000
#&gt; GSM49602     1  0.0000      0.986 1.000 0.000
#&gt; GSM49603     1  0.0000      0.986 1.000 0.000
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-1-a').click(function(){
  $('#tab-CV-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-2'>
<p><a id='tab-CV-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM49613     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM49604     2  0.4974      0.754 0.000 0.764 0.236
#&gt; GSM49605     2  0.0000      0.899 0.000 1.000 0.000
#&gt; GSM49606     2  0.0000      0.899 0.000 1.000 0.000
#&gt; GSM49607     2  0.0000      0.899 0.000 1.000 0.000
#&gt; GSM49608     2  0.0000      0.899 0.000 1.000 0.000
#&gt; GSM49609     2  0.0000      0.899 0.000 1.000 0.000
#&gt; GSM49610     2  0.0000      0.899 0.000 1.000 0.000
#&gt; GSM49611     2  0.0000      0.899 0.000 1.000 0.000
#&gt; GSM49612     2  0.0000      0.899 0.000 1.000 0.000
#&gt; GSM49614     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM49615     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM49616     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM49617     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM49564     1  0.1529      0.962 0.960 0.000 0.040
#&gt; GSM49565     1  0.0000      0.988 1.000 0.000 0.000
#&gt; GSM49566     1  0.0892      0.976 0.980 0.000 0.020
#&gt; GSM49567     1  0.0000      0.988 1.000 0.000 0.000
#&gt; GSM49568     1  0.0000      0.988 1.000 0.000 0.000
#&gt; GSM49569     1  0.1289      0.968 0.968 0.000 0.032
#&gt; GSM49570     2  0.5650      0.663 0.000 0.688 0.312
#&gt; GSM49571     1  0.4452      0.752 0.808 0.000 0.192
#&gt; GSM49572     1  0.0000      0.988 1.000 0.000 0.000
#&gt; GSM49573     2  0.5831      0.695 0.008 0.708 0.284
#&gt; GSM49574     1  0.0000      0.988 1.000 0.000 0.000
#&gt; GSM49575     1  0.0000      0.988 1.000 0.000 0.000
#&gt; GSM49576     1  0.1411      0.965 0.964 0.000 0.036
#&gt; GSM49577     1  0.0000      0.988 1.000 0.000 0.000
#&gt; GSM49578     1  0.0000      0.988 1.000 0.000 0.000
#&gt; GSM49579     1  0.0000      0.988 1.000 0.000 0.000
#&gt; GSM49580     1  0.0000      0.988 1.000 0.000 0.000
#&gt; GSM49581     1  0.0000      0.988 1.000 0.000 0.000
#&gt; GSM49582     1  0.0000      0.988 1.000 0.000 0.000
#&gt; GSM49583     2  0.1753      0.880 0.000 0.952 0.048
#&gt; GSM49584     1  0.0000      0.988 1.000 0.000 0.000
#&gt; GSM49585     1  0.0000      0.988 1.000 0.000 0.000
#&gt; GSM49586     1  0.0000      0.988 1.000 0.000 0.000
#&gt; GSM49587     1  0.0000      0.988 1.000 0.000 0.000
#&gt; GSM49588     1  0.0000      0.988 1.000 0.000 0.000
#&gt; GSM49589     1  0.1411      0.965 0.964 0.000 0.036
#&gt; GSM49590     1  0.1411      0.965 0.964 0.000 0.036
#&gt; GSM49591     1  0.0424      0.984 0.992 0.000 0.008
#&gt; GSM49592     1  0.0592      0.982 0.988 0.000 0.012
#&gt; GSM49593     1  0.0000      0.988 1.000 0.000 0.000
#&gt; GSM49594     1  0.0000      0.988 1.000 0.000 0.000
#&gt; GSM49595     1  0.0000      0.988 1.000 0.000 0.000
#&gt; GSM49596     1  0.0000      0.988 1.000 0.000 0.000
#&gt; GSM49597     2  0.5016      0.751 0.000 0.760 0.240
#&gt; GSM49598     1  0.0000      0.988 1.000 0.000 0.000
#&gt; GSM49599     1  0.0000      0.988 1.000 0.000 0.000
#&gt; GSM49600     1  0.0000      0.988 1.000 0.000 0.000
#&gt; GSM49601     1  0.0000      0.988 1.000 0.000 0.000
#&gt; GSM49602     1  0.0237      0.986 0.996 0.000 0.004
#&gt; GSM49603     1  0.0424      0.984 0.992 0.000 0.008
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-2-a').click(function(){
  $('#tab-CV-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-3'>
<p><a id='tab-CV-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM49613     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM49604     2  0.4941     -0.526 0.000 0.564 0.000 0.436
#&gt; GSM49605     2  0.0336      0.849 0.000 0.992 0.000 0.008
#&gt; GSM49606     2  0.0000      0.855 0.000 1.000 0.000 0.000
#&gt; GSM49607     2  0.0000      0.855 0.000 1.000 0.000 0.000
#&gt; GSM49608     2  0.2408      0.732 0.000 0.896 0.000 0.104
#&gt; GSM49609     2  0.0000      0.855 0.000 1.000 0.000 0.000
#&gt; GSM49610     2  0.0000      0.855 0.000 1.000 0.000 0.000
#&gt; GSM49611     2  0.0000      0.855 0.000 1.000 0.000 0.000
#&gt; GSM49612     2  0.0000      0.855 0.000 1.000 0.000 0.000
#&gt; GSM49614     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM49615     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM49616     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM49617     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM49564     1  0.1114      0.834 0.972 0.008 0.004 0.016
#&gt; GSM49565     1  0.4804      0.750 0.616 0.000 0.000 0.384
#&gt; GSM49566     1  0.0779      0.844 0.980 0.000 0.004 0.016
#&gt; GSM49567     1  0.4948      0.718 0.560 0.000 0.000 0.440
#&gt; GSM49568     1  0.3219      0.834 0.836 0.000 0.000 0.164
#&gt; GSM49569     1  0.0657      0.837 0.984 0.000 0.004 0.012
#&gt; GSM49570     4  0.6532      0.934 0.000 0.368 0.084 0.548
#&gt; GSM49571     1  0.5233      0.725 0.580 0.004 0.004 0.412
#&gt; GSM49572     1  0.4877      0.737 0.592 0.000 0.000 0.408
#&gt; GSM49573     4  0.5775      0.885 0.000 0.408 0.032 0.560
#&gt; GSM49574     1  0.4830      0.746 0.608 0.000 0.000 0.392
#&gt; GSM49575     1  0.4948      0.718 0.560 0.000 0.000 0.440
#&gt; GSM49576     1  0.0657      0.837 0.984 0.000 0.004 0.012
#&gt; GSM49577     1  0.4877      0.737 0.592 0.000 0.000 0.408
#&gt; GSM49578     1  0.3649      0.828 0.796 0.000 0.000 0.204
#&gt; GSM49579     1  0.0000      0.842 1.000 0.000 0.000 0.000
#&gt; GSM49580     1  0.2868      0.841 0.864 0.000 0.000 0.136
#&gt; GSM49581     1  0.4713      0.767 0.640 0.000 0.000 0.360
#&gt; GSM49582     1  0.4193      0.805 0.732 0.000 0.000 0.268
#&gt; GSM49583     2  0.3908      0.486 0.000 0.784 0.004 0.212
#&gt; GSM49584     1  0.1637      0.843 0.940 0.000 0.000 0.060
#&gt; GSM49585     1  0.0188      0.842 0.996 0.000 0.000 0.004
#&gt; GSM49586     1  0.4643      0.765 0.656 0.000 0.000 0.344
#&gt; GSM49587     1  0.3726      0.826 0.788 0.000 0.000 0.212
#&gt; GSM49588     1  0.0000      0.842 1.000 0.000 0.000 0.000
#&gt; GSM49589     1  0.0992      0.834 0.976 0.008 0.004 0.012
#&gt; GSM49590     1  0.0992      0.834 0.976 0.008 0.004 0.012
#&gt; GSM49591     1  0.3444      0.834 0.816 0.000 0.000 0.184
#&gt; GSM49592     1  0.2921      0.840 0.860 0.000 0.000 0.140
#&gt; GSM49593     1  0.0469      0.838 0.988 0.000 0.000 0.012
#&gt; GSM49594     1  0.4164      0.793 0.736 0.000 0.000 0.264
#&gt; GSM49595     1  0.3764      0.806 0.784 0.000 0.000 0.216
#&gt; GSM49596     1  0.0336      0.840 0.992 0.000 0.000 0.008
#&gt; GSM49597     4  0.6443      0.940 0.000 0.376 0.076 0.548
#&gt; GSM49598     1  0.1118      0.836 0.964 0.000 0.000 0.036
#&gt; GSM49599     1  0.4941      0.721 0.564 0.000 0.000 0.436
#&gt; GSM49600     1  0.1474      0.839 0.948 0.000 0.000 0.052
#&gt; GSM49601     1  0.0921      0.841 0.972 0.000 0.000 0.028
#&gt; GSM49602     1  0.0469      0.838 0.988 0.000 0.000 0.012
#&gt; GSM49603     1  0.0657      0.837 0.984 0.000 0.004 0.012
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-3-a').click(function(){
  $('#tab-CV-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-4'>
<p><a id='tab-CV-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2 p3    p4    p5
#&gt; GSM49613     3  0.0000     1.0000 0.000 0.000  1 0.000 0.000
#&gt; GSM49604     4  0.4300    -0.1186 0.000 0.476  0 0.524 0.000
#&gt; GSM49605     2  0.0609     0.8638 0.000 0.980  0 0.020 0.000
#&gt; GSM49606     2  0.0000     0.8758 0.000 1.000  0 0.000 0.000
#&gt; GSM49607     2  0.0000     0.8758 0.000 1.000  0 0.000 0.000
#&gt; GSM49608     2  0.3796     0.4865 0.000 0.700  0 0.300 0.000
#&gt; GSM49609     2  0.0000     0.8758 0.000 1.000  0 0.000 0.000
#&gt; GSM49610     2  0.0162     0.8738 0.000 0.996  0 0.004 0.000
#&gt; GSM49611     2  0.0000     0.8758 0.000 1.000  0 0.000 0.000
#&gt; GSM49612     2  0.0000     0.8758 0.000 1.000  0 0.000 0.000
#&gt; GSM49614     3  0.0000     1.0000 0.000 0.000  1 0.000 0.000
#&gt; GSM49615     3  0.0000     1.0000 0.000 0.000  1 0.000 0.000
#&gt; GSM49616     3  0.0000     1.0000 0.000 0.000  1 0.000 0.000
#&gt; GSM49617     3  0.0000     1.0000 0.000 0.000  1 0.000 0.000
#&gt; GSM49564     5  0.1197     0.7197 0.048 0.000  0 0.000 0.952
#&gt; GSM49565     1  0.4171     0.5468 0.604 0.000  0 0.000 0.396
#&gt; GSM49566     5  0.0451     0.7438 0.008 0.000  0 0.004 0.988
#&gt; GSM49567     1  0.2377     0.6150 0.872 0.000  0 0.000 0.128
#&gt; GSM49568     5  0.4074     0.2555 0.364 0.000  0 0.000 0.636
#&gt; GSM49569     5  0.0000     0.7430 0.000 0.000  0 0.000 1.000
#&gt; GSM49570     4  0.0162     0.8068 0.000 0.004  0 0.996 0.000
#&gt; GSM49571     1  0.1285     0.5644 0.956 0.004  0 0.004 0.036
#&gt; GSM49572     1  0.4161     0.5419 0.608 0.000  0 0.000 0.392
#&gt; GSM49573     4  0.0162     0.8068 0.000 0.004  0 0.996 0.000
#&gt; GSM49574     1  0.4150     0.5505 0.612 0.000  0 0.000 0.388
#&gt; GSM49575     1  0.0162     0.5625 0.996 0.000  0 0.000 0.004
#&gt; GSM49576     5  0.0162     0.7419 0.004 0.000  0 0.000 0.996
#&gt; GSM49577     1  0.3876     0.5984 0.684 0.000  0 0.000 0.316
#&gt; GSM49578     5  0.3949     0.3087 0.332 0.000  0 0.000 0.668
#&gt; GSM49579     5  0.0671     0.7433 0.016 0.000  0 0.004 0.980
#&gt; GSM49580     5  0.4390     0.2594 0.428 0.000  0 0.004 0.568
#&gt; GSM49581     1  0.3857     0.3543 0.688 0.000  0 0.000 0.312
#&gt; GSM49582     1  0.4256     0.1042 0.564 0.000  0 0.000 0.436
#&gt; GSM49583     2  0.4304    -0.0609 0.000 0.516  0 0.484 0.000
#&gt; GSM49584     5  0.3336     0.5652 0.228 0.000  0 0.000 0.772
#&gt; GSM49585     5  0.2389     0.6774 0.116 0.000  0 0.004 0.880
#&gt; GSM49586     1  0.3814     0.5841 0.720 0.000  0 0.004 0.276
#&gt; GSM49587     5  0.4114     0.2097 0.376 0.000  0 0.000 0.624
#&gt; GSM49588     5  0.1671     0.7214 0.076 0.000  0 0.000 0.924
#&gt; GSM49589     5  0.0162     0.7419 0.004 0.000  0 0.000 0.996
#&gt; GSM49590     5  0.0162     0.7419 0.004 0.000  0 0.000 0.996
#&gt; GSM49591     5  0.3857     0.3427 0.312 0.000  0 0.000 0.688
#&gt; GSM49592     5  0.3876     0.3423 0.316 0.000  0 0.000 0.684
#&gt; GSM49593     5  0.1864     0.7186 0.068 0.004  0 0.004 0.924
#&gt; GSM49594     1  0.4594     0.4146 0.508 0.004  0 0.004 0.484
#&gt; GSM49595     1  0.4452     0.3944 0.500 0.000  0 0.004 0.496
#&gt; GSM49596     5  0.1544     0.7280 0.068 0.000  0 0.000 0.932
#&gt; GSM49597     4  0.0290     0.8053 0.000 0.008  0 0.992 0.000
#&gt; GSM49598     5  0.3906     0.4371 0.292 0.000  0 0.004 0.704
#&gt; GSM49599     1  0.0162     0.5625 0.996 0.000  0 0.000 0.004
#&gt; GSM49600     5  0.4135     0.3868 0.340 0.000  0 0.004 0.656
#&gt; GSM49601     5  0.3569     0.6411 0.152 0.028  0 0.004 0.816
#&gt; GSM49602     5  0.0162     0.7436 0.004 0.000  0 0.000 0.996
#&gt; GSM49603     5  0.0000     0.7430 0.000 0.000  0 0.000 1.000
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-4-a').click(function(){
  $('#tab-CV-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-5'>
<p><a id='tab-CV-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2 p3    p4    p5    p6
#&gt; GSM49613     3  0.0000     1.0000 0.000 0.000  1 0.000 0.000 0.000
#&gt; GSM49604     2  0.3966     0.2089 0.000 0.552  0 0.444 0.000 0.004
#&gt; GSM49605     2  0.0260     0.7597 0.000 0.992  0 0.008 0.000 0.000
#&gt; GSM49606     2  0.1663     0.7548 0.000 0.912  0 0.000 0.000 0.088
#&gt; GSM49607     2  0.0000     0.7608 0.000 1.000  0 0.000 0.000 0.000
#&gt; GSM49608     2  0.2730     0.6346 0.000 0.808  0 0.192 0.000 0.000
#&gt; GSM49609     2  0.3151     0.7096 0.000 0.748  0 0.000 0.000 0.252
#&gt; GSM49610     2  0.0146     0.7607 0.000 0.996  0 0.004 0.000 0.000
#&gt; GSM49611     2  0.3151     0.7096 0.000 0.748  0 0.000 0.000 0.252
#&gt; GSM49612     2  0.3151     0.7096 0.000 0.748  0 0.000 0.000 0.252
#&gt; GSM49614     3  0.0000     1.0000 0.000 0.000  1 0.000 0.000 0.000
#&gt; GSM49615     3  0.0000     1.0000 0.000 0.000  1 0.000 0.000 0.000
#&gt; GSM49616     3  0.0000     1.0000 0.000 0.000  1 0.000 0.000 0.000
#&gt; GSM49617     3  0.0000     1.0000 0.000 0.000  1 0.000 0.000 0.000
#&gt; GSM49564     5  0.3151     0.5133 0.000 0.000  0 0.000 0.748 0.252
#&gt; GSM49565     1  0.4863    -0.2357 0.528 0.000  0 0.000 0.060 0.412
#&gt; GSM49566     5  0.2790     0.5782 0.024 0.000  0 0.000 0.844 0.132
#&gt; GSM49567     1  0.2854     0.2837 0.792 0.000  0 0.000 0.000 0.208
#&gt; GSM49568     5  0.6023    -0.2227 0.320 0.000  0 0.000 0.420 0.260
#&gt; GSM49569     5  0.1895     0.5846 0.016 0.000  0 0.000 0.912 0.072
#&gt; GSM49570     4  0.0000     0.9911 0.000 0.000  0 1.000 0.000 0.000
#&gt; GSM49571     1  0.2190     0.3487 0.900 0.000  0 0.000 0.040 0.060
#&gt; GSM49572     1  0.4409    -0.0920 0.588 0.000  0 0.000 0.032 0.380
#&gt; GSM49573     4  0.0000     0.9911 0.000 0.000  0 1.000 0.000 0.000
#&gt; GSM49574     1  0.5029    -0.2675 0.524 0.000  0 0.000 0.076 0.400
#&gt; GSM49575     1  0.0000     0.3949 1.000 0.000  0 0.000 0.000 0.000
#&gt; GSM49576     5  0.3050     0.5184 0.000 0.000  0 0.000 0.764 0.236
#&gt; GSM49577     1  0.3984    -0.0663 0.596 0.000  0 0.000 0.008 0.396
#&gt; GSM49578     1  0.5795    -0.0553 0.476 0.000  0 0.000 0.328 0.196
#&gt; GSM49579     5  0.2949     0.5606 0.028 0.000  0 0.000 0.832 0.140
#&gt; GSM49580     1  0.4283     0.0298 0.592 0.000  0 0.000 0.384 0.024
#&gt; GSM49581     1  0.2106     0.3920 0.904 0.000  0 0.000 0.064 0.032
#&gt; GSM49582     1  0.4252     0.2587 0.652 0.000  0 0.000 0.312 0.036
#&gt; GSM49583     2  0.3979     0.1818 0.000 0.540  0 0.456 0.000 0.004
#&gt; GSM49584     5  0.3619     0.4761 0.232 0.000  0 0.000 0.744 0.024
#&gt; GSM49585     5  0.4253     0.4042 0.044 0.000  0 0.000 0.672 0.284
#&gt; GSM49586     1  0.4986    -0.1773 0.600 0.000  0 0.000 0.096 0.304
#&gt; GSM49587     1  0.5922    -0.3274 0.432 0.000  0 0.000 0.216 0.352
#&gt; GSM49588     5  0.4153     0.3386 0.024 0.000  0 0.000 0.636 0.340
#&gt; GSM49589     5  0.3126     0.5144 0.000 0.000  0 0.000 0.752 0.248
#&gt; GSM49590     5  0.3126     0.5144 0.000 0.000  0 0.000 0.752 0.248
#&gt; GSM49591     1  0.5956    -0.1864 0.420 0.000  0 0.000 0.356 0.224
#&gt; GSM49592     5  0.6015    -0.4213 0.376 0.000  0 0.000 0.384 0.240
#&gt; GSM49593     5  0.4301     0.2344 0.024 0.000  0 0.000 0.584 0.392
#&gt; GSM49594     6  0.5879     0.9452 0.344 0.000  0 0.000 0.208 0.448
#&gt; GSM49595     6  0.5932     0.9463 0.336 0.000  0 0.000 0.224 0.440
#&gt; GSM49596     5  0.4028     0.3932 0.024 0.000  0 0.000 0.668 0.308
#&gt; GSM49597     4  0.0603     0.9819 0.000 0.016  0 0.980 0.000 0.004
#&gt; GSM49598     5  0.3778     0.4626 0.288 0.000  0 0.000 0.696 0.016
#&gt; GSM49599     1  0.0000     0.3949 1.000 0.000  0 0.000 0.000 0.000
#&gt; GSM49600     5  0.3738     0.4777 0.280 0.000  0 0.000 0.704 0.016
#&gt; GSM49601     5  0.4573     0.2451 0.044 0.000  0 0.000 0.584 0.372
#&gt; GSM49602     5  0.2709     0.5669 0.020 0.000  0 0.000 0.848 0.132
#&gt; GSM49603     5  0.1092     0.5933 0.020 0.000  0 0.000 0.960 0.020
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-5-a').click(function(){
  $('#tab-CV-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-CV-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-get-signatures'>
<ul>
<li><a href='#tab-CV-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-1-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-1"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-2-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-2"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-3-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-3"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-4-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-4"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-5-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-mclust-signature_compare](figure_cola/CV-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-CV-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-mclust-collect-classes](figure_cola/CV-mclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n disease.state(p) cell.type(p) k
#> CV:mclust 53         6.04e-06     2.65e-05 2
#> CV:mclust 54         2.57e-07     8.46e-14 3
#> CV:mclust 52         4.64e-10     2.91e-14 4
#> CV:mclust 39         7.13e-07     7.13e-10 5
#> CV:mclust 27         1.05e-04     9.31e-06 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:NMF






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "NMF"]
# you can also extract it by
# res = res_list["CV:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21168 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-NMF-collect-plots](figure_cola/CV-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-NMF-select-partition-number](figure_cola/CV-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.887           0.937       0.972         0.4214 0.591   0.591
#> 3 3 0.896           0.891       0.949         0.3945 0.781   0.642
#> 4 4 0.642           0.804       0.885         0.1323 0.944   0.866
#> 5 5 0.591           0.560       0.772         0.1073 0.888   0.713
#> 6 6 0.747           0.762       0.860         0.0902 0.826   0.482
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-get-classes'>
<ul>
<li><a href='#tab-CV-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-NMF-get-classes-1'>
<p><a id='tab-CV-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM49613     1  0.0000      0.967 1.000 0.000
#&gt; GSM49604     2  0.0000      0.975 0.000 1.000
#&gt; GSM49605     2  0.0000      0.975 0.000 1.000
#&gt; GSM49606     2  0.0000      0.975 0.000 1.000
#&gt; GSM49607     2  0.0000      0.975 0.000 1.000
#&gt; GSM49608     2  0.0000      0.975 0.000 1.000
#&gt; GSM49609     2  0.0000      0.975 0.000 1.000
#&gt; GSM49610     2  0.0000      0.975 0.000 1.000
#&gt; GSM49611     2  0.0000      0.975 0.000 1.000
#&gt; GSM49612     2  0.0000      0.975 0.000 1.000
#&gt; GSM49614     1  0.9323      0.473 0.652 0.348
#&gt; GSM49615     1  0.0000      0.967 1.000 0.000
#&gt; GSM49616     1  0.0000      0.967 1.000 0.000
#&gt; GSM49617     1  0.0000      0.967 1.000 0.000
#&gt; GSM49564     1  0.0000      0.967 1.000 0.000
#&gt; GSM49565     1  0.5294      0.861 0.880 0.120
#&gt; GSM49566     1  0.0000      0.967 1.000 0.000
#&gt; GSM49567     1  0.1184      0.957 0.984 0.016
#&gt; GSM49568     1  0.0000      0.967 1.000 0.000
#&gt; GSM49569     1  0.0000      0.967 1.000 0.000
#&gt; GSM49570     2  0.0000      0.975 0.000 1.000
#&gt; GSM49571     2  0.1633      0.954 0.024 0.976
#&gt; GSM49572     1  0.0000      0.967 1.000 0.000
#&gt; GSM49573     2  0.0000      0.975 0.000 1.000
#&gt; GSM49574     1  0.2948      0.927 0.948 0.052
#&gt; GSM49575     1  0.1843      0.947 0.972 0.028
#&gt; GSM49576     1  0.0000      0.967 1.000 0.000
#&gt; GSM49577     2  0.8813      0.544 0.300 0.700
#&gt; GSM49578     1  0.0000      0.967 1.000 0.000
#&gt; GSM49579     1  0.0000      0.967 1.000 0.000
#&gt; GSM49580     1  0.0000      0.967 1.000 0.000
#&gt; GSM49581     1  0.0000      0.967 1.000 0.000
#&gt; GSM49582     1  0.0000      0.967 1.000 0.000
#&gt; GSM49583     2  0.0000      0.975 0.000 1.000
#&gt; GSM49584     1  0.0000      0.967 1.000 0.000
#&gt; GSM49585     1  0.0000      0.967 1.000 0.000
#&gt; GSM49586     1  0.7453      0.744 0.788 0.212
#&gt; GSM49587     1  0.0000      0.967 1.000 0.000
#&gt; GSM49588     1  0.0000      0.967 1.000 0.000
#&gt; GSM49589     1  0.0000      0.967 1.000 0.000
#&gt; GSM49590     1  0.0000      0.967 1.000 0.000
#&gt; GSM49591     1  0.0000      0.967 1.000 0.000
#&gt; GSM49592     1  0.0000      0.967 1.000 0.000
#&gt; GSM49593     1  0.0000      0.967 1.000 0.000
#&gt; GSM49594     1  0.7602      0.737 0.780 0.220
#&gt; GSM49595     1  0.0376      0.965 0.996 0.004
#&gt; GSM49596     1  0.0000      0.967 1.000 0.000
#&gt; GSM49597     2  0.0000      0.975 0.000 1.000
#&gt; GSM49598     1  0.0000      0.967 1.000 0.000
#&gt; GSM49599     1  0.7139      0.760 0.804 0.196
#&gt; GSM49600     1  0.0000      0.967 1.000 0.000
#&gt; GSM49601     1  0.0000      0.967 1.000 0.000
#&gt; GSM49602     1  0.0000      0.967 1.000 0.000
#&gt; GSM49603     1  0.0000      0.967 1.000 0.000
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-1-a').click(function(){
  $('#tab-CV-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-2'>
<p><a id='tab-CV-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM49613     3  0.1031      0.854 0.024 0.000 0.976
#&gt; GSM49604     2  0.0237      0.969 0.000 0.996 0.004
#&gt; GSM49605     2  0.0592      0.969 0.000 0.988 0.012
#&gt; GSM49606     2  0.0000      0.968 0.000 1.000 0.000
#&gt; GSM49607     2  0.0592      0.969 0.000 0.988 0.012
#&gt; GSM49608     2  0.0747      0.967 0.000 0.984 0.016
#&gt; GSM49609     2  0.0592      0.962 0.000 0.988 0.012
#&gt; GSM49610     2  0.0592      0.969 0.000 0.988 0.012
#&gt; GSM49611     2  0.0592      0.962 0.000 0.988 0.012
#&gt; GSM49612     2  0.0424      0.965 0.000 0.992 0.008
#&gt; GSM49614     3  0.0892      0.837 0.000 0.020 0.980
#&gt; GSM49615     3  0.1031      0.854 0.024 0.000 0.976
#&gt; GSM49616     3  0.0661      0.846 0.004 0.008 0.988
#&gt; GSM49617     3  0.0592      0.843 0.000 0.012 0.988
#&gt; GSM49564     3  0.1643      0.851 0.044 0.000 0.956
#&gt; GSM49565     1  0.1620      0.935 0.964 0.024 0.012
#&gt; GSM49566     1  0.2711      0.883 0.912 0.000 0.088
#&gt; GSM49567     1  0.1482      0.937 0.968 0.020 0.012
#&gt; GSM49568     1  0.0000      0.946 1.000 0.000 0.000
#&gt; GSM49569     1  0.2796      0.879 0.908 0.000 0.092
#&gt; GSM49570     2  0.4291      0.799 0.000 0.820 0.180
#&gt; GSM49571     2  0.3031      0.872 0.076 0.912 0.012
#&gt; GSM49572     1  0.1015      0.942 0.980 0.012 0.008
#&gt; GSM49573     2  0.0592      0.969 0.000 0.988 0.012
#&gt; GSM49574     1  0.1482      0.937 0.968 0.020 0.012
#&gt; GSM49575     1  0.1482      0.937 0.968 0.020 0.012
#&gt; GSM49576     3  0.6307      0.131 0.488 0.000 0.512
#&gt; GSM49577     1  0.6647      0.360 0.592 0.396 0.012
#&gt; GSM49578     1  0.0000      0.946 1.000 0.000 0.000
#&gt; GSM49579     1  0.0237      0.946 0.996 0.000 0.004
#&gt; GSM49580     1  0.1753      0.923 0.952 0.000 0.048
#&gt; GSM49581     1  0.0000      0.946 1.000 0.000 0.000
#&gt; GSM49582     1  0.0000      0.946 1.000 0.000 0.000
#&gt; GSM49583     2  0.0237      0.969 0.000 0.996 0.004
#&gt; GSM49584     1  0.1289      0.934 0.968 0.000 0.032
#&gt; GSM49585     1  0.0000      0.946 1.000 0.000 0.000
#&gt; GSM49586     1  0.2749      0.905 0.924 0.064 0.012
#&gt; GSM49587     1  0.0237      0.946 0.996 0.004 0.000
#&gt; GSM49588     1  0.0000      0.946 1.000 0.000 0.000
#&gt; GSM49589     3  0.5254      0.672 0.264 0.000 0.736
#&gt; GSM49590     3  0.2959      0.823 0.100 0.000 0.900
#&gt; GSM49591     1  0.1015      0.942 0.980 0.012 0.008
#&gt; GSM49592     1  0.0475      0.945 0.992 0.004 0.004
#&gt; GSM49593     1  0.0747      0.941 0.984 0.000 0.016
#&gt; GSM49594     1  0.2749      0.904 0.924 0.064 0.012
#&gt; GSM49595     1  0.1182      0.940 0.976 0.012 0.012
#&gt; GSM49596     1  0.0000      0.946 1.000 0.000 0.000
#&gt; GSM49597     2  0.0592      0.969 0.000 0.988 0.012
#&gt; GSM49598     1  0.1163      0.936 0.972 0.000 0.028
#&gt; GSM49599     1  0.5378      0.675 0.756 0.236 0.008
#&gt; GSM49600     1  0.1643      0.925 0.956 0.000 0.044
#&gt; GSM49601     1  0.0000      0.946 1.000 0.000 0.000
#&gt; GSM49602     1  0.0237      0.946 0.996 0.000 0.004
#&gt; GSM49603     1  0.1289      0.934 0.968 0.000 0.032
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-2-a').click(function(){
  $('#tab-CV-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-3'>
<p><a id='tab-CV-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM49613     3  0.0469      0.868 0.012 0.000 0.988 0.000
#&gt; GSM49604     4  0.4624      0.474 0.000 0.340 0.000 0.660
#&gt; GSM49605     2  0.0921      0.909 0.000 0.972 0.000 0.028
#&gt; GSM49606     2  0.0469      0.908 0.000 0.988 0.000 0.012
#&gt; GSM49607     2  0.2401      0.883 0.000 0.904 0.004 0.092
#&gt; GSM49608     2  0.2197      0.892 0.000 0.916 0.004 0.080
#&gt; GSM49609     2  0.0469      0.899 0.000 0.988 0.000 0.012
#&gt; GSM49610     2  0.2125      0.897 0.000 0.920 0.004 0.076
#&gt; GSM49611     2  0.0336      0.903 0.000 0.992 0.000 0.008
#&gt; GSM49612     2  0.0188      0.905 0.000 0.996 0.000 0.004
#&gt; GSM49614     3  0.2944      0.753 0.000 0.004 0.868 0.128
#&gt; GSM49615     3  0.0188      0.867 0.004 0.000 0.996 0.000
#&gt; GSM49616     3  0.0336      0.862 0.000 0.000 0.992 0.008
#&gt; GSM49617     3  0.1004      0.850 0.000 0.004 0.972 0.024
#&gt; GSM49564     3  0.0779      0.868 0.016 0.000 0.980 0.004
#&gt; GSM49565     1  0.1629      0.879 0.952 0.024 0.000 0.024
#&gt; GSM49566     1  0.3737      0.826 0.840 0.004 0.136 0.020
#&gt; GSM49567     1  0.3123      0.803 0.844 0.000 0.000 0.156
#&gt; GSM49568     1  0.0657      0.882 0.984 0.000 0.004 0.012
#&gt; GSM49569     1  0.4323      0.773 0.788 0.000 0.184 0.028
#&gt; GSM49570     4  0.2775      0.748 0.000 0.084 0.020 0.896
#&gt; GSM49571     4  0.4711      0.686 0.064 0.152 0.000 0.784
#&gt; GSM49572     1  0.1474      0.873 0.948 0.000 0.000 0.052
#&gt; GSM49573     4  0.2310      0.751 0.004 0.068 0.008 0.920
#&gt; GSM49574     1  0.1042      0.880 0.972 0.008 0.000 0.020
#&gt; GSM49575     1  0.3873      0.733 0.772 0.000 0.000 0.228
#&gt; GSM49576     3  0.5440      0.263 0.384 0.000 0.596 0.020
#&gt; GSM49577     1  0.4800      0.708 0.760 0.044 0.000 0.196
#&gt; GSM49578     1  0.0779      0.882 0.980 0.000 0.004 0.016
#&gt; GSM49579     1  0.1510      0.881 0.956 0.000 0.016 0.028
#&gt; GSM49580     1  0.3972      0.752 0.788 0.000 0.008 0.204
#&gt; GSM49581     1  0.2345      0.849 0.900 0.000 0.000 0.100
#&gt; GSM49582     1  0.0895      0.881 0.976 0.000 0.004 0.020
#&gt; GSM49583     2  0.4761      0.505 0.000 0.664 0.004 0.332
#&gt; GSM49584     1  0.0672      0.883 0.984 0.000 0.008 0.008
#&gt; GSM49585     1  0.1975      0.882 0.936 0.000 0.016 0.048
#&gt; GSM49586     1  0.6449      0.610 0.644 0.152 0.000 0.204
#&gt; GSM49587     1  0.0592      0.881 0.984 0.000 0.000 0.016
#&gt; GSM49588     1  0.1520      0.882 0.956 0.000 0.020 0.024
#&gt; GSM49589     3  0.2500      0.838 0.040 0.000 0.916 0.044
#&gt; GSM49590     3  0.2300      0.840 0.048 0.000 0.924 0.028
#&gt; GSM49591     1  0.1022      0.884 0.968 0.000 0.000 0.032
#&gt; GSM49592     1  0.1443      0.885 0.960 0.008 0.004 0.028
#&gt; GSM49593     1  0.3697      0.839 0.852 0.000 0.100 0.048
#&gt; GSM49594     1  0.5792      0.598 0.648 0.296 0.000 0.056
#&gt; GSM49595     1  0.4261      0.817 0.820 0.112 0.000 0.068
#&gt; GSM49596     1  0.1706      0.880 0.948 0.000 0.016 0.036
#&gt; GSM49597     4  0.3725      0.701 0.000 0.180 0.008 0.812
#&gt; GSM49598     1  0.1743      0.882 0.940 0.000 0.004 0.056
#&gt; GSM49599     4  0.4331      0.534 0.288 0.000 0.000 0.712
#&gt; GSM49600     1  0.0657      0.883 0.984 0.000 0.004 0.012
#&gt; GSM49601     1  0.6375      0.754 0.728 0.088 0.096 0.088
#&gt; GSM49602     1  0.3691      0.847 0.856 0.000 0.076 0.068
#&gt; GSM49603     1  0.4764      0.802 0.788 0.000 0.124 0.088
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-3-a').click(function(){
  $('#tab-CV-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-4'>
<p><a id='tab-CV-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM49613     3  0.0000   0.877078 0.000 0.000 1.000 0.000 0.000
#&gt; GSM49604     4  0.5849   0.595904 0.000 0.100 0.000 0.508 0.392
#&gt; GSM49605     2  0.0955   0.899861 0.000 0.968 0.000 0.028 0.004
#&gt; GSM49606     2  0.0324   0.904032 0.000 0.992 0.000 0.004 0.004
#&gt; GSM49607     2  0.3895   0.683424 0.004 0.728 0.000 0.264 0.004
#&gt; GSM49608     2  0.2929   0.815322 0.000 0.840 0.000 0.152 0.008
#&gt; GSM49609     2  0.1671   0.868660 0.000 0.924 0.000 0.000 0.076
#&gt; GSM49610     2  0.0671   0.903760 0.000 0.980 0.000 0.016 0.004
#&gt; GSM49611     2  0.1270   0.887584 0.000 0.948 0.000 0.000 0.052
#&gt; GSM49612     2  0.0609   0.901172 0.000 0.980 0.000 0.000 0.020
#&gt; GSM49614     3  0.2522   0.801520 0.000 0.000 0.880 0.108 0.012
#&gt; GSM49615     3  0.0000   0.877078 0.000 0.000 1.000 0.000 0.000
#&gt; GSM49616     3  0.0324   0.874830 0.000 0.000 0.992 0.004 0.004
#&gt; GSM49617     3  0.1168   0.859329 0.000 0.000 0.960 0.032 0.008
#&gt; GSM49564     3  0.0451   0.874853 0.004 0.000 0.988 0.000 0.008
#&gt; GSM49565     1  0.1485   0.691143 0.948 0.000 0.000 0.032 0.020
#&gt; GSM49566     1  0.6565   0.440232 0.604 0.004 0.048 0.112 0.232
#&gt; GSM49567     1  0.3238   0.627958 0.836 0.000 0.000 0.136 0.028
#&gt; GSM49568     1  0.0609   0.698868 0.980 0.000 0.000 0.000 0.020
#&gt; GSM49569     1  0.6089   0.300142 0.568 0.000 0.124 0.008 0.300
#&gt; GSM49570     4  0.4317   0.634045 0.000 0.004 0.008 0.668 0.320
#&gt; GSM49571     5  0.5322  -0.571697 0.012 0.036 0.000 0.372 0.580
#&gt; GSM49572     1  0.2712   0.658872 0.880 0.000 0.000 0.088 0.032
#&gt; GSM49573     4  0.4516   0.597391 0.000 0.004 0.004 0.576 0.416
#&gt; GSM49574     1  0.0898   0.695984 0.972 0.000 0.000 0.008 0.020
#&gt; GSM49575     1  0.4138   0.544783 0.780 0.000 0.000 0.072 0.148
#&gt; GSM49576     1  0.7011   0.001922 0.452 0.000 0.248 0.016 0.284
#&gt; GSM49577     1  0.6107   0.339626 0.584 0.032 0.000 0.308 0.076
#&gt; GSM49578     1  0.0794   0.694908 0.972 0.000 0.000 0.000 0.028
#&gt; GSM49579     1  0.4423   0.493957 0.684 0.000 0.008 0.012 0.296
#&gt; GSM49580     1  0.3474   0.657423 0.836 0.000 0.004 0.116 0.044
#&gt; GSM49581     1  0.1582   0.700584 0.944 0.000 0.000 0.028 0.028
#&gt; GSM49582     1  0.1082   0.696372 0.964 0.000 0.000 0.008 0.028
#&gt; GSM49583     4  0.4710  -0.000644 0.012 0.364 0.000 0.616 0.008
#&gt; GSM49584     1  0.0671   0.698882 0.980 0.000 0.004 0.000 0.016
#&gt; GSM49585     1  0.4065   0.508606 0.720 0.000 0.016 0.000 0.264
#&gt; GSM49586     5  0.2790   0.053419 0.028 0.020 0.000 0.060 0.892
#&gt; GSM49587     1  0.0324   0.698436 0.992 0.000 0.000 0.004 0.004
#&gt; GSM49588     1  0.2520   0.678579 0.888 0.000 0.012 0.004 0.096
#&gt; GSM49589     3  0.3769   0.709476 0.028 0.000 0.796 0.004 0.172
#&gt; GSM49590     3  0.4723   0.521515 0.032 0.000 0.688 0.008 0.272
#&gt; GSM49591     1  0.2280   0.669924 0.880 0.000 0.000 0.000 0.120
#&gt; GSM49592     1  0.2329   0.662398 0.876 0.000 0.000 0.000 0.124
#&gt; GSM49593     1  0.5582   0.258429 0.544 0.000 0.056 0.008 0.392
#&gt; GSM49594     5  0.6060   0.096517 0.384 0.124 0.000 0.000 0.492
#&gt; GSM49595     1  0.5193   0.041232 0.484 0.032 0.004 0.000 0.480
#&gt; GSM49596     1  0.4280   0.491236 0.676 0.000 0.004 0.008 0.312
#&gt; GSM49597     4  0.2228   0.539361 0.004 0.092 0.000 0.900 0.004
#&gt; GSM49598     1  0.4876   0.255916 0.544 0.000 0.012 0.008 0.436
#&gt; GSM49599     4  0.5476   0.488003 0.160 0.000 0.000 0.656 0.184
#&gt; GSM49600     1  0.2463   0.679671 0.888 0.000 0.004 0.008 0.100
#&gt; GSM49601     5  0.6970   0.296744 0.324 0.056 0.116 0.000 0.504
#&gt; GSM49602     1  0.5236   0.095379 0.492 0.000 0.044 0.000 0.464
#&gt; GSM49603     5  0.5965   0.253056 0.328 0.000 0.128 0.000 0.544
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-4-a').click(function(){
  $('#tab-CV-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-5'>
<p><a id='tab-CV-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM49613     3  0.0363      0.919 0.000 0.000 0.988 0.000 0.012 0.000
#&gt; GSM49604     4  0.2420      0.727 0.008 0.044 0.000 0.904 0.016 0.028
#&gt; GSM49605     2  0.2531      0.773 0.000 0.856 0.000 0.000 0.012 0.132
#&gt; GSM49606     2  0.1531      0.802 0.000 0.928 0.000 0.004 0.000 0.068
#&gt; GSM49607     2  0.3989      0.253 0.000 0.528 0.000 0.000 0.004 0.468
#&gt; GSM49608     2  0.3835      0.571 0.000 0.668 0.000 0.000 0.012 0.320
#&gt; GSM49609     2  0.2103      0.759 0.000 0.912 0.000 0.020 0.012 0.056
#&gt; GSM49610     2  0.1477      0.807 0.000 0.940 0.000 0.008 0.004 0.048
#&gt; GSM49611     2  0.1585      0.779 0.000 0.940 0.000 0.012 0.012 0.036
#&gt; GSM49612     2  0.0436      0.802 0.000 0.988 0.000 0.004 0.004 0.004
#&gt; GSM49614     3  0.1969      0.868 0.000 0.004 0.920 0.020 0.004 0.052
#&gt; GSM49615     3  0.0260      0.920 0.000 0.000 0.992 0.000 0.008 0.000
#&gt; GSM49616     3  0.0146      0.920 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; GSM49617     3  0.0146      0.917 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; GSM49564     3  0.0964      0.914 0.000 0.000 0.968 0.012 0.016 0.004
#&gt; GSM49565     1  0.2382      0.877 0.896 0.008 0.000 0.004 0.020 0.072
#&gt; GSM49566     5  0.5315      0.500 0.124 0.004 0.004 0.000 0.612 0.256
#&gt; GSM49567     1  0.3161      0.831 0.828 0.000 0.000 0.008 0.028 0.136
#&gt; GSM49568     1  0.0837      0.893 0.972 0.004 0.000 0.004 0.020 0.000
#&gt; GSM49569     5  0.2867      0.851 0.064 0.000 0.040 0.000 0.872 0.024
#&gt; GSM49570     4  0.2454      0.690 0.000 0.000 0.000 0.840 0.000 0.160
#&gt; GSM49571     4  0.2570      0.718 0.012 0.012 0.000 0.896 0.032 0.048
#&gt; GSM49572     1  0.2911      0.854 0.856 0.000 0.000 0.008 0.036 0.100
#&gt; GSM49573     4  0.1787      0.730 0.008 0.000 0.000 0.920 0.004 0.068
#&gt; GSM49574     1  0.1198      0.891 0.960 0.004 0.000 0.004 0.012 0.020
#&gt; GSM49575     1  0.2380      0.847 0.892 0.000 0.000 0.068 0.004 0.036
#&gt; GSM49576     5  0.2467      0.842 0.020 0.000 0.036 0.000 0.896 0.048
#&gt; GSM49577     6  0.6248      0.434 0.136 0.044 0.000 0.008 0.252 0.560
#&gt; GSM49578     1  0.0912      0.891 0.972 0.004 0.000 0.004 0.012 0.008
#&gt; GSM49579     5  0.3159      0.814 0.068 0.000 0.000 0.000 0.832 0.100
#&gt; GSM49580     1  0.4441      0.765 0.768 0.000 0.000 0.068 0.080 0.084
#&gt; GSM49581     1  0.1908      0.888 0.924 0.000 0.000 0.012 0.044 0.020
#&gt; GSM49582     1  0.0622      0.892 0.980 0.000 0.000 0.000 0.012 0.008
#&gt; GSM49583     6  0.3850      0.459 0.004 0.196 0.000 0.024 0.012 0.764
#&gt; GSM49584     1  0.1391      0.890 0.944 0.000 0.000 0.000 0.040 0.016
#&gt; GSM49585     1  0.3834      0.786 0.812 0.008 0.000 0.032 0.108 0.040
#&gt; GSM49586     4  0.5678      0.443 0.012 0.032 0.000 0.612 0.264 0.080
#&gt; GSM49587     1  0.1148      0.892 0.960 0.000 0.000 0.004 0.020 0.016
#&gt; GSM49588     1  0.2056      0.874 0.904 0.000 0.000 0.004 0.080 0.012
#&gt; GSM49589     3  0.3198      0.604 0.000 0.000 0.740 0.000 0.260 0.000
#&gt; GSM49590     5  0.2358      0.802 0.000 0.000 0.108 0.000 0.876 0.016
#&gt; GSM49591     1  0.2007      0.878 0.916 0.000 0.000 0.004 0.044 0.036
#&gt; GSM49592     1  0.1465      0.884 0.948 0.004 0.000 0.004 0.020 0.024
#&gt; GSM49593     5  0.2006      0.855 0.080 0.000 0.016 0.000 0.904 0.000
#&gt; GSM49594     5  0.1592      0.838 0.012 0.016 0.000 0.004 0.944 0.024
#&gt; GSM49595     5  0.1526      0.853 0.036 0.004 0.000 0.008 0.944 0.008
#&gt; GSM49596     5  0.2915      0.759 0.184 0.000 0.000 0.000 0.808 0.008
#&gt; GSM49597     6  0.4323      0.509 0.000 0.044 0.000 0.148 0.048 0.760
#&gt; GSM49598     5  0.2062      0.850 0.088 0.000 0.000 0.008 0.900 0.004
#&gt; GSM49599     4  0.5302      0.442 0.172 0.000 0.000 0.616 0.004 0.208
#&gt; GSM49600     1  0.4343      0.338 0.592 0.000 0.000 0.004 0.384 0.020
#&gt; GSM49601     5  0.6460      0.578 0.056 0.088 0.056 0.044 0.668 0.088
#&gt; GSM49602     5  0.1452      0.857 0.032 0.000 0.008 0.004 0.948 0.008
#&gt; GSM49603     5  0.2368      0.843 0.028 0.000 0.036 0.020 0.908 0.008
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-5-a').click(function(){
  $('#tab-CV-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-CV-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-get-signatures'>
<ul>
<li><a href='#tab-CV-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-1-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-1"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-2-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-2"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-3-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-3"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-4-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-4"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-5-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-NMF-signature_compare](figure_cola/CV-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-CV-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-NMF-collect-classes](figure_cola/CV-NMF-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>         n disease.state(p) cell.type(p) k
#> CV:NMF 53         9.92e-06     2.43e-03 2
#> CV:NMF 52         2.66e-06     1.99e-08 3
#> CV:NMF 52         1.25e-08     3.47e-09 4
#> CV:NMF 37         2.51e-06     2.20e-06 5
#> CV:NMF 47         7.14e-07     1.63e-08 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:hclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "hclust"]
# you can also extract it by
# res = res_list["MAD:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21168 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 4.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-hclust-collect-plots](figure_cola/MAD-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-hclust-select-partition-number](figure_cola/MAD-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.926           0.955       0.978         0.3780 0.609   0.609
#> 3 3 0.964           0.944       0.980         0.3329 0.878   0.799
#> 4 4 0.972           0.916       0.969         0.0681 0.969   0.935
#> 5 5 0.696           0.809       0.889         0.1076 0.991   0.980
#> 6 6 0.676           0.699       0.753         0.1601 0.776   0.521
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 4
#> attr(,"optional")
#> [1] 2 3
```

There is also optional best $k$ = 2 3 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-get-classes'>
<ul>
<li><a href='#tab-MAD-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-hclust-get-classes-1'>
<p><a id='tab-MAD-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM49613     1  0.0672      0.987 0.992 0.008
#&gt; GSM49604     2  0.3584      0.909 0.068 0.932
#&gt; GSM49605     2  0.0672      0.937 0.008 0.992
#&gt; GSM49606     2  0.0672      0.937 0.008 0.992
#&gt; GSM49607     2  0.0672      0.937 0.008 0.992
#&gt; GSM49608     2  0.0672      0.937 0.008 0.992
#&gt; GSM49609     2  0.0672      0.937 0.008 0.992
#&gt; GSM49610     2  0.0672      0.937 0.008 0.992
#&gt; GSM49611     2  0.0672      0.937 0.008 0.992
#&gt; GSM49612     2  0.0672      0.937 0.008 0.992
#&gt; GSM49614     1  0.0672      0.987 0.992 0.008
#&gt; GSM49615     1  0.0672      0.987 0.992 0.008
#&gt; GSM49616     1  0.0672      0.987 0.992 0.008
#&gt; GSM49617     1  0.0672      0.987 0.992 0.008
#&gt; GSM49564     1  0.0000      0.993 1.000 0.000
#&gt; GSM49565     1  0.0000      0.993 1.000 0.000
#&gt; GSM49566     1  0.0000      0.993 1.000 0.000
#&gt; GSM49567     1  0.0000      0.993 1.000 0.000
#&gt; GSM49568     1  0.0000      0.993 1.000 0.000
#&gt; GSM49569     1  0.0000      0.993 1.000 0.000
#&gt; GSM49570     2  0.5408      0.864 0.124 0.876
#&gt; GSM49571     2  0.9983      0.152 0.476 0.524
#&gt; GSM49572     1  0.0000      0.993 1.000 0.000
#&gt; GSM49573     2  0.5408      0.864 0.124 0.876
#&gt; GSM49574     1  0.0000      0.993 1.000 0.000
#&gt; GSM49575     1  0.3114      0.934 0.944 0.056
#&gt; GSM49576     1  0.0000      0.993 1.000 0.000
#&gt; GSM49577     1  0.0000      0.993 1.000 0.000
#&gt; GSM49578     1  0.0000      0.993 1.000 0.000
#&gt; GSM49579     1  0.0000      0.993 1.000 0.000
#&gt; GSM49580     1  0.0000      0.993 1.000 0.000
#&gt; GSM49581     1  0.0000      0.993 1.000 0.000
#&gt; GSM49582     1  0.0000      0.993 1.000 0.000
#&gt; GSM49583     2  0.0672      0.937 0.008 0.992
#&gt; GSM49584     1  0.0000      0.993 1.000 0.000
#&gt; GSM49585     1  0.0000      0.993 1.000 0.000
#&gt; GSM49586     1  0.6343      0.793 0.840 0.160
#&gt; GSM49587     1  0.0000      0.993 1.000 0.000
#&gt; GSM49588     1  0.0000      0.993 1.000 0.000
#&gt; GSM49589     1  0.0000      0.993 1.000 0.000
#&gt; GSM49590     1  0.0000      0.993 1.000 0.000
#&gt; GSM49591     1  0.0000      0.993 1.000 0.000
#&gt; GSM49592     1  0.0000      0.993 1.000 0.000
#&gt; GSM49593     1  0.0000      0.993 1.000 0.000
#&gt; GSM49594     1  0.0000      0.993 1.000 0.000
#&gt; GSM49595     1  0.0000      0.993 1.000 0.000
#&gt; GSM49596     1  0.0000      0.993 1.000 0.000
#&gt; GSM49597     2  0.2778      0.921 0.048 0.952
#&gt; GSM49598     1  0.0000      0.993 1.000 0.000
#&gt; GSM49599     1  0.0000      0.993 1.000 0.000
#&gt; GSM49600     1  0.0000      0.993 1.000 0.000
#&gt; GSM49601     1  0.0000      0.993 1.000 0.000
#&gt; GSM49602     1  0.0000      0.993 1.000 0.000
#&gt; GSM49603     1  0.0000      0.993 1.000 0.000
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-1-a').click(function(){
  $('#tab-MAD-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-2'>
<p><a id='tab-MAD-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM49613     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM49604     2  0.2165      0.853 0.064 0.936 0.000
#&gt; GSM49605     2  0.0000      0.896 0.000 1.000 0.000
#&gt; GSM49606     2  0.0000      0.896 0.000 1.000 0.000
#&gt; GSM49607     2  0.0000      0.896 0.000 1.000 0.000
#&gt; GSM49608     2  0.0000      0.896 0.000 1.000 0.000
#&gt; GSM49609     2  0.0000      0.896 0.000 1.000 0.000
#&gt; GSM49610     2  0.0000      0.896 0.000 1.000 0.000
#&gt; GSM49611     2  0.0000      0.896 0.000 1.000 0.000
#&gt; GSM49612     2  0.0000      0.896 0.000 1.000 0.000
#&gt; GSM49614     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM49615     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM49616     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM49617     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM49564     1  0.0424      0.986 0.992 0.000 0.008
#&gt; GSM49565     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM49566     1  0.0237      0.989 0.996 0.000 0.004
#&gt; GSM49567     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM49568     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM49569     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM49570     2  0.3412      0.790 0.124 0.876 0.000
#&gt; GSM49571     2  0.6299      0.138 0.476 0.524 0.000
#&gt; GSM49572     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM49573     2  0.3412      0.790 0.124 0.876 0.000
#&gt; GSM49574     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM49575     1  0.1964      0.934 0.944 0.056 0.000
#&gt; GSM49576     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM49577     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM49578     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM49579     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM49580     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM49581     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM49582     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM49583     2  0.0000      0.896 0.000 1.000 0.000
#&gt; GSM49584     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM49585     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM49586     1  0.4002      0.795 0.840 0.160 0.000
#&gt; GSM49587     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM49588     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM49589     1  0.0237      0.989 0.996 0.000 0.004
#&gt; GSM49590     1  0.0592      0.983 0.988 0.000 0.012
#&gt; GSM49591     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM49592     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM49593     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM49594     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM49595     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM49596     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM49597     2  0.1529      0.873 0.040 0.960 0.000
#&gt; GSM49598     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM49599     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM49600     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM49601     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM49602     1  0.0000      0.992 1.000 0.000 0.000
#&gt; GSM49603     1  0.0000      0.992 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-2-a').click(function(){
  $('#tab-MAD-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-3'>
<p><a id='tab-MAD-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM49613     3  0.0000     1.0000 0.000 0.000 1.000 0.000
#&gt; GSM49604     4  0.4996     0.0198 0.000 0.484 0.000 0.516
#&gt; GSM49605     2  0.0000     0.9910 0.000 1.000 0.000 0.000
#&gt; GSM49606     2  0.0000     0.9910 0.000 1.000 0.000 0.000
#&gt; GSM49607     2  0.0000     0.9910 0.000 1.000 0.000 0.000
#&gt; GSM49608     2  0.0000     0.9910 0.000 1.000 0.000 0.000
#&gt; GSM49609     2  0.0000     0.9910 0.000 1.000 0.000 0.000
#&gt; GSM49610     2  0.0000     0.9910 0.000 1.000 0.000 0.000
#&gt; GSM49611     2  0.0000     0.9910 0.000 1.000 0.000 0.000
#&gt; GSM49612     2  0.0000     0.9910 0.000 1.000 0.000 0.000
#&gt; GSM49614     3  0.0000     1.0000 0.000 0.000 1.000 0.000
#&gt; GSM49615     3  0.0000     1.0000 0.000 0.000 1.000 0.000
#&gt; GSM49616     3  0.0000     1.0000 0.000 0.000 1.000 0.000
#&gt; GSM49617     3  0.0000     1.0000 0.000 0.000 1.000 0.000
#&gt; GSM49564     1  0.0336     0.9845 0.992 0.000 0.008 0.000
#&gt; GSM49565     1  0.0000     0.9912 1.000 0.000 0.000 0.000
#&gt; GSM49566     1  0.0188     0.9880 0.996 0.000 0.004 0.000
#&gt; GSM49567     1  0.0000     0.9912 1.000 0.000 0.000 0.000
#&gt; GSM49568     1  0.0000     0.9912 1.000 0.000 0.000 0.000
#&gt; GSM49569     1  0.0000     0.9912 1.000 0.000 0.000 0.000
#&gt; GSM49570     4  0.0000     0.4791 0.000 0.000 0.000 1.000
#&gt; GSM49571     4  0.5193     0.2269 0.412 0.008 0.000 0.580
#&gt; GSM49572     1  0.0000     0.9912 1.000 0.000 0.000 0.000
#&gt; GSM49573     4  0.0000     0.4791 0.000 0.000 0.000 1.000
#&gt; GSM49574     1  0.0000     0.9912 1.000 0.000 0.000 0.000
#&gt; GSM49575     1  0.1637     0.9276 0.940 0.000 0.000 0.060
#&gt; GSM49576     1  0.0000     0.9912 1.000 0.000 0.000 0.000
#&gt; GSM49577     1  0.0000     0.9912 1.000 0.000 0.000 0.000
#&gt; GSM49578     1  0.0000     0.9912 1.000 0.000 0.000 0.000
#&gt; GSM49579     1  0.0000     0.9912 1.000 0.000 0.000 0.000
#&gt; GSM49580     1  0.0000     0.9912 1.000 0.000 0.000 0.000
#&gt; GSM49581     1  0.0000     0.9912 1.000 0.000 0.000 0.000
#&gt; GSM49582     1  0.0000     0.9912 1.000 0.000 0.000 0.000
#&gt; GSM49583     2  0.1557     0.9236 0.000 0.944 0.000 0.056
#&gt; GSM49584     1  0.0000     0.9912 1.000 0.000 0.000 0.000
#&gt; GSM49585     1  0.0000     0.9912 1.000 0.000 0.000 0.000
#&gt; GSM49586     1  0.3444     0.7553 0.816 0.000 0.000 0.184
#&gt; GSM49587     1  0.0000     0.9912 1.000 0.000 0.000 0.000
#&gt; GSM49588     1  0.0000     0.9912 1.000 0.000 0.000 0.000
#&gt; GSM49589     1  0.0188     0.9880 0.996 0.000 0.004 0.000
#&gt; GSM49590     1  0.0469     0.9806 0.988 0.000 0.012 0.000
#&gt; GSM49591     1  0.0000     0.9912 1.000 0.000 0.000 0.000
#&gt; GSM49592     1  0.0000     0.9912 1.000 0.000 0.000 0.000
#&gt; GSM49593     1  0.0000     0.9912 1.000 0.000 0.000 0.000
#&gt; GSM49594     1  0.0000     0.9912 1.000 0.000 0.000 0.000
#&gt; GSM49595     1  0.0000     0.9912 1.000 0.000 0.000 0.000
#&gt; GSM49596     1  0.0000     0.9912 1.000 0.000 0.000 0.000
#&gt; GSM49597     4  0.4981     0.0447 0.000 0.464 0.000 0.536
#&gt; GSM49598     1  0.0000     0.9912 1.000 0.000 0.000 0.000
#&gt; GSM49599     1  0.0000     0.9912 1.000 0.000 0.000 0.000
#&gt; GSM49600     1  0.0000     0.9912 1.000 0.000 0.000 0.000
#&gt; GSM49601     1  0.0000     0.9912 1.000 0.000 0.000 0.000
#&gt; GSM49602     1  0.0000     0.9912 1.000 0.000 0.000 0.000
#&gt; GSM49603     1  0.0000     0.9912 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-3-a').click(function(){
  $('#tab-MAD-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-4'>
<p><a id='tab-MAD-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM49613     3  0.0000    1.00000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM49604     4  0.4656   -0.19963 0.000 0.480 0.000 0.508 0.012
#&gt; GSM49605     2  0.0000    1.00000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49606     2  0.0000    1.00000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49607     2  0.0000    1.00000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49608     2  0.0000    1.00000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49609     2  0.0000    1.00000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49610     2  0.0000    1.00000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49611     2  0.0000    1.00000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49612     2  0.0000    1.00000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49614     3  0.0000    1.00000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM49615     3  0.0000    1.00000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM49616     3  0.0000    1.00000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM49617     3  0.0000    1.00000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM49564     1  0.3242    0.85539 0.784 0.000 0.000 0.000 0.216
#&gt; GSM49565     1  0.1197    0.88917 0.952 0.000 0.000 0.000 0.048
#&gt; GSM49566     1  0.3210    0.86120 0.788 0.000 0.000 0.000 0.212
#&gt; GSM49567     1  0.0703    0.88483 0.976 0.000 0.000 0.000 0.024
#&gt; GSM49568     1  0.0609    0.88369 0.980 0.000 0.000 0.000 0.020
#&gt; GSM49569     1  0.3039    0.86527 0.808 0.000 0.000 0.000 0.192
#&gt; GSM49570     4  0.3274   -0.00937 0.000 0.000 0.000 0.780 0.220
#&gt; GSM49571     4  0.4621    0.11500 0.412 0.008 0.000 0.576 0.004
#&gt; GSM49572     1  0.0609    0.88369 0.980 0.000 0.000 0.000 0.020
#&gt; GSM49573     4  0.0000    0.11999 0.000 0.000 0.000 1.000 0.000
#&gt; GSM49574     1  0.0609    0.88369 0.980 0.000 0.000 0.000 0.020
#&gt; GSM49575     1  0.2012    0.84484 0.920 0.000 0.000 0.060 0.020
#&gt; GSM49576     1  0.3143    0.86047 0.796 0.000 0.000 0.000 0.204
#&gt; GSM49577     1  0.1908    0.88938 0.908 0.000 0.000 0.000 0.092
#&gt; GSM49578     1  0.0609    0.88369 0.980 0.000 0.000 0.000 0.020
#&gt; GSM49579     1  0.3003    0.86877 0.812 0.000 0.000 0.000 0.188
#&gt; GSM49580     1  0.0963    0.88934 0.964 0.000 0.000 0.000 0.036
#&gt; GSM49581     1  0.0880    0.88875 0.968 0.000 0.000 0.000 0.032
#&gt; GSM49582     1  0.0609    0.88369 0.980 0.000 0.000 0.000 0.020
#&gt; GSM49583     5  0.5077    0.16664 0.000 0.428 0.000 0.036 0.536
#&gt; GSM49584     1  0.0963    0.88934 0.964 0.000 0.000 0.000 0.036
#&gt; GSM49585     1  0.0963    0.88935 0.964 0.000 0.000 0.000 0.036
#&gt; GSM49586     1  0.4948    0.66806 0.708 0.000 0.000 0.184 0.108
#&gt; GSM49587     1  0.0609    0.88369 0.980 0.000 0.000 0.000 0.020
#&gt; GSM49588     1  0.0880    0.88656 0.968 0.000 0.000 0.000 0.032
#&gt; GSM49589     1  0.3074    0.86574 0.804 0.000 0.000 0.000 0.196
#&gt; GSM49590     1  0.3607    0.83299 0.752 0.000 0.004 0.000 0.244
#&gt; GSM49591     1  0.0609    0.88369 0.980 0.000 0.000 0.000 0.020
#&gt; GSM49592     1  0.0609    0.88369 0.980 0.000 0.000 0.000 0.020
#&gt; GSM49593     1  0.3074    0.86337 0.804 0.000 0.000 0.000 0.196
#&gt; GSM49594     1  0.2424    0.88202 0.868 0.000 0.000 0.000 0.132
#&gt; GSM49595     1  0.2424    0.88202 0.868 0.000 0.000 0.000 0.132
#&gt; GSM49596     1  0.3074    0.86337 0.804 0.000 0.000 0.000 0.196
#&gt; GSM49597     5  0.3876    0.10995 0.000 0.000 0.000 0.316 0.684
#&gt; GSM49598     1  0.3177    0.85862 0.792 0.000 0.000 0.000 0.208
#&gt; GSM49599     1  0.0609    0.88369 0.980 0.000 0.000 0.000 0.020
#&gt; GSM49600     1  0.0963    0.88913 0.964 0.000 0.000 0.000 0.036
#&gt; GSM49601     1  0.3109    0.86114 0.800 0.000 0.000 0.000 0.200
#&gt; GSM49602     1  0.3177    0.85660 0.792 0.000 0.000 0.000 0.208
#&gt; GSM49603     1  0.3177    0.85660 0.792 0.000 0.000 0.000 0.208
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-4-a').click(function(){
  $('#tab-MAD-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-5'>
<p><a id='tab-MAD-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2 p3    p4    p5    p6
#&gt; GSM49613     3  0.0000    1.00000 0.000 0.000  1 0.000 0.000 0.000
#&gt; GSM49604     2  0.7091    0.00351 0.000 0.468  0 0.212 0.160 0.160
#&gt; GSM49605     2  0.0146    0.91528 0.000 0.996  0 0.000 0.004 0.000
#&gt; GSM49606     2  0.0000    0.91645 0.000 1.000  0 0.000 0.000 0.000
#&gt; GSM49607     2  0.0146    0.91528 0.000 0.996  0 0.000 0.004 0.000
#&gt; GSM49608     2  0.0146    0.91528 0.000 0.996  0 0.000 0.004 0.000
#&gt; GSM49609     2  0.0000    0.91645 0.000 1.000  0 0.000 0.000 0.000
#&gt; GSM49610     2  0.0000    0.91645 0.000 1.000  0 0.000 0.000 0.000
#&gt; GSM49611     2  0.0000    0.91645 0.000 1.000  0 0.000 0.000 0.000
#&gt; GSM49612     2  0.0000    0.91645 0.000 1.000  0 0.000 0.000 0.000
#&gt; GSM49614     3  0.0000    1.00000 0.000 0.000  1 0.000 0.000 0.000
#&gt; GSM49615     3  0.0000    1.00000 0.000 0.000  1 0.000 0.000 0.000
#&gt; GSM49616     3  0.0000    1.00000 0.000 0.000  1 0.000 0.000 0.000
#&gt; GSM49617     3  0.0000    1.00000 0.000 0.000  1 0.000 0.000 0.000
#&gt; GSM49564     5  0.3769    0.90483 0.356 0.000  0 0.000 0.640 0.004
#&gt; GSM49565     1  0.1610    0.74313 0.916 0.000  0 0.000 0.084 0.000
#&gt; GSM49566     5  0.3795    0.90155 0.364 0.000  0 0.000 0.632 0.004
#&gt; GSM49567     1  0.1663    0.74150 0.912 0.000  0 0.000 0.088 0.000
#&gt; GSM49568     1  0.0146    0.78225 0.996 0.000  0 0.000 0.004 0.000
#&gt; GSM49569     5  0.3819    0.91562 0.372 0.000  0 0.000 0.624 0.004
#&gt; GSM49570     4  0.1204    0.28759 0.000 0.000  0 0.944 0.000 0.056
#&gt; GSM49571     1  0.7490   -0.21496 0.384 0.000  0 0.212 0.192 0.212
#&gt; GSM49572     1  0.0291    0.78130 0.992 0.000  0 0.000 0.004 0.004
#&gt; GSM49573     4  0.5406    0.45295 0.000 0.000  0 0.568 0.160 0.272
#&gt; GSM49574     1  0.0405    0.78115 0.988 0.000  0 0.000 0.008 0.004
#&gt; GSM49575     1  0.2136    0.72110 0.904 0.000  0 0.000 0.048 0.048
#&gt; GSM49576     5  0.3728    0.90821 0.344 0.000  0 0.000 0.652 0.004
#&gt; GSM49577     1  0.3133    0.53229 0.780 0.000  0 0.000 0.212 0.008
#&gt; GSM49578     1  0.0000    0.78149 1.000 0.000  0 0.000 0.000 0.000
#&gt; GSM49579     5  0.3672    0.91044 0.368 0.000  0 0.000 0.632 0.000
#&gt; GSM49580     1  0.1714    0.74142 0.908 0.000  0 0.000 0.092 0.000
#&gt; GSM49581     1  0.1714    0.74611 0.908 0.000  0 0.000 0.092 0.000
#&gt; GSM49582     1  0.0000    0.78149 1.000 0.000  0 0.000 0.000 0.000
#&gt; GSM49583     6  0.3351    0.32655 0.000 0.288  0 0.000 0.000 0.712
#&gt; GSM49584     1  0.1387    0.75830 0.932 0.000  0 0.000 0.068 0.000
#&gt; GSM49585     1  0.2482    0.65367 0.848 0.000  0 0.000 0.148 0.004
#&gt; GSM49586     1  0.5119    0.00167 0.496 0.000  0 0.008 0.436 0.060
#&gt; GSM49587     1  0.0146    0.78134 0.996 0.000  0 0.000 0.000 0.004
#&gt; GSM49588     1  0.1285    0.76662 0.944 0.000  0 0.000 0.052 0.004
#&gt; GSM49589     5  0.3841    0.90115 0.380 0.000  0 0.000 0.616 0.004
#&gt; GSM49590     5  0.3584    0.87900 0.308 0.000  0 0.000 0.688 0.004
#&gt; GSM49591     1  0.0692    0.77984 0.976 0.000  0 0.000 0.020 0.004
#&gt; GSM49592     1  0.0260    0.78167 0.992 0.000  0 0.000 0.008 0.000
#&gt; GSM49593     5  0.3819    0.91499 0.372 0.000  0 0.000 0.624 0.004
#&gt; GSM49594     1  0.3998   -0.66706 0.504 0.000  0 0.000 0.492 0.004
#&gt; GSM49595     1  0.3998   -0.66706 0.504 0.000  0 0.000 0.492 0.004
#&gt; GSM49596     5  0.3923    0.87315 0.416 0.000  0 0.000 0.580 0.004
#&gt; GSM49597     6  0.3864    0.06634 0.000 0.000  0 0.480 0.000 0.520
#&gt; GSM49598     5  0.3782    0.86011 0.412 0.000  0 0.000 0.588 0.000
#&gt; GSM49599     1  0.0260    0.78144 0.992 0.000  0 0.000 0.008 0.000
#&gt; GSM49600     1  0.1714    0.74191 0.908 0.000  0 0.000 0.092 0.000
#&gt; GSM49601     5  0.3852    0.90662 0.384 0.000  0 0.000 0.612 0.004
#&gt; GSM49602     5  0.3862    0.89262 0.388 0.000  0 0.000 0.608 0.004
#&gt; GSM49603     5  0.3862    0.89262 0.388 0.000  0 0.000 0.608 0.004
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-5-a').click(function(){
  $('#tab-MAD-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-get-signatures'>
<ul>
<li><a href='#tab-MAD-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-1-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-1"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-2-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-2"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-3-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-3"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-4-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-4"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-5-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-hclust-signature_compare](figure_cola/MAD-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-hclust-collect-classes](figure_cola/MAD-hclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n disease.state(p) cell.type(p) k
#> MAD:hclust 53         8.04e-07     7.74e-04 2
#> MAD:hclust 53         3.58e-07     1.61e-13 3
#> MAD:hclust 49         6.39e-09     5.84e-14 4
#> MAD:hclust 48         5.21e-10     2.84e-14 5
#> MAD:hclust 45         1.07e-08     4.31e-12 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:kmeans*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "kmeans"]
# you can also extract it by
# res = res_list["MAD:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21168 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 5.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-kmeans-collect-plots](figure_cola/MAD-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-kmeans-select-partition-number](figure_cola/MAD-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.547           0.964       0.946         0.3414 0.628   0.628
#> 3 3 0.709           0.885       0.920         0.5585 0.874   0.800
#> 4 4 0.723           0.908       0.830         0.2410 0.774   0.549
#> 5 5 0.919           0.950       0.929         0.1328 0.960   0.857
#> 6 6 0.805           0.862       0.872         0.0552 1.000   1.000
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 5
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-get-classes'>
<ul>
<li><a href='#tab-MAD-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-kmeans-get-classes-1'>
<p><a id='tab-MAD-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM49613     1  0.6247      0.826 0.844 0.156
#&gt; GSM49604     2  0.6247      0.992 0.156 0.844
#&gt; GSM49605     2  0.6247      0.992 0.156 0.844
#&gt; GSM49606     2  0.6247      0.992 0.156 0.844
#&gt; GSM49607     2  0.6247      0.992 0.156 0.844
#&gt; GSM49608     2  0.6247      0.992 0.156 0.844
#&gt; GSM49609     2  0.6247      0.992 0.156 0.844
#&gt; GSM49610     2  0.6247      0.992 0.156 0.844
#&gt; GSM49611     2  0.6247      0.992 0.156 0.844
#&gt; GSM49612     2  0.6247      0.992 0.156 0.844
#&gt; GSM49614     1  0.6247      0.826 0.844 0.156
#&gt; GSM49615     1  0.6247      0.826 0.844 0.156
#&gt; GSM49616     1  0.6247      0.826 0.844 0.156
#&gt; GSM49617     1  0.6247      0.826 0.844 0.156
#&gt; GSM49564     1  0.0376      0.973 0.996 0.004
#&gt; GSM49565     1  0.0000      0.976 1.000 0.000
#&gt; GSM49566     1  0.0000      0.976 1.000 0.000
#&gt; GSM49567     1  0.0000      0.976 1.000 0.000
#&gt; GSM49568     1  0.0000      0.976 1.000 0.000
#&gt; GSM49569     1  0.0000      0.976 1.000 0.000
#&gt; GSM49570     2  0.6531      0.978 0.168 0.832
#&gt; GSM49571     1  0.1414      0.956 0.980 0.020
#&gt; GSM49572     1  0.0000      0.976 1.000 0.000
#&gt; GSM49573     2  0.7139      0.954 0.196 0.804
#&gt; GSM49574     1  0.0000      0.976 1.000 0.000
#&gt; GSM49575     1  0.0000      0.976 1.000 0.000
#&gt; GSM49576     1  0.0376      0.973 0.996 0.004
#&gt; GSM49577     1  0.0000      0.976 1.000 0.000
#&gt; GSM49578     1  0.0000      0.976 1.000 0.000
#&gt; GSM49579     1  0.0000      0.976 1.000 0.000
#&gt; GSM49580     1  0.0000      0.976 1.000 0.000
#&gt; GSM49581     1  0.0000      0.976 1.000 0.000
#&gt; GSM49582     1  0.0000      0.976 1.000 0.000
#&gt; GSM49583     2  0.6247      0.992 0.156 0.844
#&gt; GSM49584     1  0.0000      0.976 1.000 0.000
#&gt; GSM49585     1  0.0000      0.976 1.000 0.000
#&gt; GSM49586     1  0.0000      0.976 1.000 0.000
#&gt; GSM49587     1  0.0000      0.976 1.000 0.000
#&gt; GSM49588     1  0.0000      0.976 1.000 0.000
#&gt; GSM49589     1  0.0376      0.973 0.996 0.004
#&gt; GSM49590     1  0.0672      0.970 0.992 0.008
#&gt; GSM49591     1  0.0000      0.976 1.000 0.000
#&gt; GSM49592     1  0.0000      0.976 1.000 0.000
#&gt; GSM49593     1  0.0000      0.976 1.000 0.000
#&gt; GSM49594     1  0.0000      0.976 1.000 0.000
#&gt; GSM49595     1  0.0000      0.976 1.000 0.000
#&gt; GSM49596     1  0.0000      0.976 1.000 0.000
#&gt; GSM49597     2  0.6531      0.978 0.168 0.832
#&gt; GSM49598     1  0.0000      0.976 1.000 0.000
#&gt; GSM49599     1  0.0000      0.976 1.000 0.000
#&gt; GSM49600     1  0.0000      0.976 1.000 0.000
#&gt; GSM49601     1  0.0000      0.976 1.000 0.000
#&gt; GSM49602     1  0.0000      0.976 1.000 0.000
#&gt; GSM49603     1  0.0000      0.976 1.000 0.000
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-1-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-2'>
<p><a id='tab-MAD-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM49613     3  0.2356      0.985 0.072 0.000 0.928
#&gt; GSM49604     2  0.1643      0.938 0.000 0.956 0.044
#&gt; GSM49605     2  0.0747      0.956 0.000 0.984 0.016
#&gt; GSM49606     2  0.0747      0.956 0.000 0.984 0.016
#&gt; GSM49607     2  0.0747      0.956 0.000 0.984 0.016
#&gt; GSM49608     2  0.0747      0.956 0.000 0.984 0.016
#&gt; GSM49609     2  0.0000      0.955 0.000 1.000 0.000
#&gt; GSM49610     2  0.0000      0.955 0.000 1.000 0.000
#&gt; GSM49611     2  0.0000      0.955 0.000 1.000 0.000
#&gt; GSM49612     2  0.0000      0.955 0.000 1.000 0.000
#&gt; GSM49614     3  0.1411      0.940 0.036 0.000 0.964
#&gt; GSM49615     3  0.2356      0.985 0.072 0.000 0.928
#&gt; GSM49616     3  0.2356      0.985 0.072 0.000 0.928
#&gt; GSM49617     3  0.2356      0.985 0.072 0.000 0.928
#&gt; GSM49564     1  0.4654      0.828 0.792 0.000 0.208
#&gt; GSM49565     1  0.0237      0.885 0.996 0.000 0.004
#&gt; GSM49566     1  0.4504      0.836 0.804 0.000 0.196
#&gt; GSM49567     1  0.0000      0.885 1.000 0.000 0.000
#&gt; GSM49568     1  0.0000      0.885 1.000 0.000 0.000
#&gt; GSM49569     1  0.4750      0.826 0.784 0.000 0.216
#&gt; GSM49570     2  0.3619      0.876 0.000 0.864 0.136
#&gt; GSM49571     1  0.5677      0.800 0.792 0.048 0.160
#&gt; GSM49572     1  0.0000      0.885 1.000 0.000 0.000
#&gt; GSM49573     2  0.4665      0.810 0.100 0.852 0.048
#&gt; GSM49574     1  0.0000      0.885 1.000 0.000 0.000
#&gt; GSM49575     1  0.0237      0.883 0.996 0.000 0.004
#&gt; GSM49576     1  0.4750      0.826 0.784 0.000 0.216
#&gt; GSM49577     1  0.0592      0.882 0.988 0.000 0.012
#&gt; GSM49578     1  0.0000      0.885 1.000 0.000 0.000
#&gt; GSM49579     1  0.4121      0.846 0.832 0.000 0.168
#&gt; GSM49580     1  0.0237      0.884 0.996 0.000 0.004
#&gt; GSM49581     1  0.0000      0.885 1.000 0.000 0.000
#&gt; GSM49582     1  0.0000      0.885 1.000 0.000 0.000
#&gt; GSM49583     2  0.0747      0.956 0.000 0.984 0.016
#&gt; GSM49584     1  0.0000      0.885 1.000 0.000 0.000
#&gt; GSM49585     1  0.0000      0.885 1.000 0.000 0.000
#&gt; GSM49586     1  0.4702      0.830 0.788 0.000 0.212
#&gt; GSM49587     1  0.0000      0.885 1.000 0.000 0.000
#&gt; GSM49588     1  0.0000      0.885 1.000 0.000 0.000
#&gt; GSM49589     1  0.4750      0.826 0.784 0.000 0.216
#&gt; GSM49590     1  0.5560      0.720 0.700 0.000 0.300
#&gt; GSM49591     1  0.0000      0.885 1.000 0.000 0.000
#&gt; GSM49592     1  0.0000      0.885 1.000 0.000 0.000
#&gt; GSM49593     1  0.4750      0.826 0.784 0.000 0.216
#&gt; GSM49594     1  0.4702      0.830 0.788 0.000 0.212
#&gt; GSM49595     1  0.4702      0.830 0.788 0.000 0.212
#&gt; GSM49596     1  0.0424      0.884 0.992 0.000 0.008
#&gt; GSM49597     2  0.3879      0.876 0.000 0.848 0.152
#&gt; GSM49598     1  0.4750      0.826 0.784 0.000 0.216
#&gt; GSM49599     1  0.0237      0.883 0.996 0.000 0.004
#&gt; GSM49600     1  0.0592      0.883 0.988 0.000 0.012
#&gt; GSM49601     1  0.4654      0.830 0.792 0.000 0.208
#&gt; GSM49602     1  0.4750      0.826 0.784 0.000 0.216
#&gt; GSM49603     1  0.4750      0.826 0.784 0.000 0.216
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-2-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-3'>
<p><a id='tab-MAD-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM49613     3  0.2334      0.993 0.004 0.000 0.908 0.088
#&gt; GSM49604     2  0.5916      0.702 0.000 0.656 0.072 0.272
#&gt; GSM49605     2  0.0000      0.862 0.000 1.000 0.000 0.000
#&gt; GSM49606     2  0.0000      0.862 0.000 1.000 0.000 0.000
#&gt; GSM49607     2  0.0000      0.862 0.000 1.000 0.000 0.000
#&gt; GSM49608     2  0.0000      0.862 0.000 1.000 0.000 0.000
#&gt; GSM49609     2  0.0804      0.862 0.000 0.980 0.008 0.012
#&gt; GSM49610     2  0.0804      0.862 0.000 0.980 0.008 0.012
#&gt; GSM49611     2  0.0804      0.862 0.000 0.980 0.008 0.012
#&gt; GSM49612     2  0.0804      0.862 0.000 0.980 0.008 0.012
#&gt; GSM49614     3  0.1978      0.988 0.004 0.000 0.928 0.068
#&gt; GSM49615     3  0.2334      0.993 0.004 0.000 0.908 0.088
#&gt; GSM49616     3  0.2125      0.994 0.004 0.000 0.920 0.076
#&gt; GSM49617     3  0.2125      0.994 0.004 0.000 0.920 0.076
#&gt; GSM49564     4  0.4624      0.931 0.340 0.000 0.000 0.660
#&gt; GSM49565     1  0.0469      0.983 0.988 0.000 0.000 0.012
#&gt; GSM49566     4  0.4624      0.936 0.340 0.000 0.000 0.660
#&gt; GSM49567     1  0.0469      0.980 0.988 0.000 0.000 0.012
#&gt; GSM49568     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49569     4  0.4661      0.940 0.348 0.000 0.000 0.652
#&gt; GSM49570     2  0.6454      0.625 0.000 0.544 0.076 0.380
#&gt; GSM49571     4  0.4726      0.401 0.108 0.012 0.072 0.808
#&gt; GSM49572     1  0.0469      0.980 0.988 0.000 0.000 0.012
#&gt; GSM49573     2  0.7456      0.594 0.036 0.508 0.080 0.376
#&gt; GSM49574     1  0.0188      0.988 0.996 0.000 0.000 0.004
#&gt; GSM49575     1  0.0188      0.988 0.996 0.000 0.000 0.004
#&gt; GSM49576     4  0.4605      0.936 0.336 0.000 0.000 0.664
#&gt; GSM49577     4  0.4925      0.814 0.428 0.000 0.000 0.572
#&gt; GSM49578     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49579     4  0.4679      0.930 0.352 0.000 0.000 0.648
#&gt; GSM49580     1  0.0188      0.986 0.996 0.000 0.000 0.004
#&gt; GSM49581     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49582     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49583     2  0.0000      0.862 0.000 1.000 0.000 0.000
#&gt; GSM49584     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49585     1  0.0336      0.986 0.992 0.000 0.000 0.008
#&gt; GSM49586     4  0.4837      0.938 0.348 0.000 0.004 0.648
#&gt; GSM49587     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49588     1  0.0188      0.986 0.996 0.000 0.000 0.004
#&gt; GSM49589     4  0.4643      0.940 0.344 0.000 0.000 0.656
#&gt; GSM49590     4  0.4605      0.936 0.336 0.000 0.000 0.664
#&gt; GSM49591     1  0.0188      0.988 0.996 0.000 0.000 0.004
#&gt; GSM49592     1  0.0188      0.988 0.996 0.000 0.000 0.004
#&gt; GSM49593     4  0.4661      0.940 0.348 0.000 0.000 0.652
#&gt; GSM49594     4  0.4643      0.939 0.344 0.000 0.000 0.656
#&gt; GSM49595     4  0.4643      0.939 0.344 0.000 0.000 0.656
#&gt; GSM49596     4  0.4967      0.792 0.452 0.000 0.000 0.548
#&gt; GSM49597     2  0.6362      0.627 0.000 0.560 0.072 0.368
#&gt; GSM49598     4  0.4661      0.940 0.348 0.000 0.000 0.652
#&gt; GSM49599     1  0.0336      0.986 0.992 0.000 0.000 0.008
#&gt; GSM49600     1  0.1557      0.910 0.944 0.000 0.000 0.056
#&gt; GSM49601     4  0.4661      0.939 0.348 0.000 0.000 0.652
#&gt; GSM49602     4  0.4661      0.940 0.348 0.000 0.000 0.652
#&gt; GSM49603     4  0.4661      0.940 0.348 0.000 0.000 0.652
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-3-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-4'>
<p><a id='tab-MAD-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM49613     3  0.0510      0.996 0.000 0.000 0.984 0.000 0.016
#&gt; GSM49604     4  0.3878      0.823 0.000 0.236 0.000 0.748 0.016
#&gt; GSM49605     2  0.0324      0.979 0.000 0.992 0.004 0.000 0.004
#&gt; GSM49606     2  0.0000      0.979 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49607     2  0.0324      0.979 0.000 0.992 0.004 0.000 0.004
#&gt; GSM49608     2  0.0324      0.979 0.000 0.992 0.004 0.000 0.004
#&gt; GSM49609     2  0.0960      0.978 0.000 0.972 0.004 0.008 0.016
#&gt; GSM49610     2  0.0960      0.978 0.000 0.972 0.004 0.008 0.016
#&gt; GSM49611     2  0.0960      0.978 0.000 0.972 0.004 0.008 0.016
#&gt; GSM49612     2  0.0960      0.978 0.000 0.972 0.004 0.008 0.016
#&gt; GSM49614     3  0.0703      0.991 0.000 0.000 0.976 0.000 0.024
#&gt; GSM49615     3  0.0510      0.996 0.000 0.000 0.984 0.000 0.016
#&gt; GSM49616     3  0.0404      0.996 0.000 0.000 0.988 0.000 0.012
#&gt; GSM49617     3  0.0404      0.996 0.000 0.000 0.988 0.000 0.012
#&gt; GSM49564     5  0.2300      0.954 0.072 0.000 0.000 0.024 0.904
#&gt; GSM49565     1  0.1892      0.946 0.916 0.000 0.000 0.080 0.004
#&gt; GSM49566     5  0.3459      0.929 0.072 0.000 0.004 0.080 0.844
#&gt; GSM49567     1  0.1443      0.953 0.948 0.000 0.004 0.044 0.004
#&gt; GSM49568     1  0.0324      0.970 0.992 0.000 0.000 0.004 0.004
#&gt; GSM49569     5  0.2228      0.958 0.076 0.000 0.004 0.012 0.908
#&gt; GSM49570     4  0.2773      0.880 0.000 0.164 0.000 0.836 0.000
#&gt; GSM49571     4  0.3318      0.646 0.008 0.000 0.000 0.800 0.192
#&gt; GSM49572     1  0.1357      0.961 0.948 0.000 0.000 0.048 0.004
#&gt; GSM49573     4  0.3183      0.879 0.000 0.156 0.000 0.828 0.016
#&gt; GSM49574     1  0.0880      0.968 0.968 0.000 0.000 0.032 0.000
#&gt; GSM49575     1  0.0912      0.965 0.972 0.000 0.000 0.016 0.012
#&gt; GSM49576     5  0.2300      0.957 0.072 0.000 0.000 0.024 0.904
#&gt; GSM49577     5  0.3859      0.897 0.096 0.000 0.004 0.084 0.816
#&gt; GSM49578     1  0.0162      0.970 0.996 0.000 0.000 0.000 0.004
#&gt; GSM49579     5  0.3012      0.948 0.072 0.000 0.004 0.052 0.872
#&gt; GSM49580     1  0.1153      0.963 0.964 0.000 0.004 0.024 0.008
#&gt; GSM49581     1  0.0833      0.967 0.976 0.000 0.004 0.016 0.004
#&gt; GSM49582     1  0.0162      0.970 0.996 0.000 0.000 0.000 0.004
#&gt; GSM49583     2  0.0968      0.966 0.000 0.972 0.004 0.012 0.012
#&gt; GSM49584     1  0.0451      0.969 0.988 0.000 0.000 0.008 0.004
#&gt; GSM49585     1  0.1697      0.952 0.932 0.000 0.000 0.060 0.008
#&gt; GSM49586     5  0.2376      0.934 0.052 0.000 0.000 0.044 0.904
#&gt; GSM49587     1  0.0865      0.969 0.972 0.000 0.000 0.024 0.004
#&gt; GSM49588     1  0.1331      0.964 0.952 0.000 0.000 0.040 0.008
#&gt; GSM49589     5  0.2853      0.958 0.072 0.000 0.000 0.052 0.876
#&gt; GSM49590     5  0.2006      0.960 0.072 0.000 0.000 0.012 0.916
#&gt; GSM49591     1  0.0963      0.966 0.964 0.000 0.000 0.036 0.000
#&gt; GSM49592     1  0.0955      0.968 0.968 0.000 0.000 0.028 0.004
#&gt; GSM49593     5  0.1956      0.959 0.076 0.000 0.000 0.008 0.916
#&gt; GSM49594     5  0.3119      0.952 0.072 0.000 0.000 0.068 0.860
#&gt; GSM49595     5  0.3119      0.952 0.072 0.000 0.000 0.068 0.860
#&gt; GSM49596     5  0.2407      0.951 0.088 0.000 0.004 0.012 0.896
#&gt; GSM49597     4  0.3399      0.874 0.000 0.172 0.004 0.812 0.012
#&gt; GSM49598     5  0.2069      0.960 0.076 0.000 0.000 0.012 0.912
#&gt; GSM49599     1  0.0671      0.969 0.980 0.000 0.004 0.016 0.000
#&gt; GSM49600     1  0.2304      0.909 0.908 0.000 0.004 0.020 0.068
#&gt; GSM49601     5  0.2843      0.954 0.076 0.000 0.000 0.048 0.876
#&gt; GSM49602     5  0.2770      0.955 0.076 0.000 0.000 0.044 0.880
#&gt; GSM49603     5  0.2694      0.955 0.076 0.000 0.000 0.040 0.884
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-4-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-5'>
<p><a id='tab-MAD-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5 p6
#&gt; GSM49613     3  0.1285      0.965 0.000 0.000 0.944 0.000 0.004 NA
#&gt; GSM49604     4  0.2699      0.806 0.000 0.124 0.000 0.856 0.008 NA
#&gt; GSM49605     2  0.1806      0.904 0.000 0.908 0.000 0.000 0.004 NA
#&gt; GSM49606     2  0.0291      0.916 0.000 0.992 0.000 0.000 0.004 NA
#&gt; GSM49607     2  0.1958      0.900 0.000 0.896 0.000 0.000 0.004 NA
#&gt; GSM49608     2  0.1918      0.904 0.000 0.904 0.000 0.000 0.008 NA
#&gt; GSM49609     2  0.1493      0.914 0.000 0.936 0.000 0.004 0.004 NA
#&gt; GSM49610     2  0.1493      0.914 0.000 0.936 0.000 0.004 0.004 NA
#&gt; GSM49611     2  0.1493      0.914 0.000 0.936 0.000 0.004 0.004 NA
#&gt; GSM49612     2  0.1493      0.914 0.000 0.936 0.000 0.004 0.004 NA
#&gt; GSM49614     3  0.0717      0.966 0.000 0.000 0.976 0.000 0.008 NA
#&gt; GSM49615     3  0.1285      0.965 0.000 0.000 0.944 0.000 0.004 NA
#&gt; GSM49616     3  0.0000      0.973 0.000 0.000 1.000 0.000 0.000 NA
#&gt; GSM49617     3  0.0000      0.973 0.000 0.000 1.000 0.000 0.000 NA
#&gt; GSM49564     5  0.2964      0.837 0.040 0.000 0.000 0.004 0.848 NA
#&gt; GSM49565     1  0.3668      0.769 0.668 0.000 0.000 0.000 0.004 NA
#&gt; GSM49566     5  0.3770      0.785 0.032 0.000 0.000 0.004 0.752 NA
#&gt; GSM49567     1  0.2980      0.808 0.800 0.000 0.000 0.000 0.008 NA
#&gt; GSM49568     1  0.0000      0.880 1.000 0.000 0.000 0.000 0.000 NA
#&gt; GSM49569     5  0.1723      0.877 0.036 0.000 0.000 0.000 0.928 NA
#&gt; GSM49570     4  0.1745      0.842 0.000 0.020 0.000 0.924 0.000 NA
#&gt; GSM49571     4  0.3974      0.707 0.004 0.000 0.000 0.752 0.056 NA
#&gt; GSM49572     1  0.3534      0.811 0.716 0.000 0.000 0.000 0.008 NA
#&gt; GSM49573     4  0.0806      0.844 0.000 0.020 0.000 0.972 0.000 NA
#&gt; GSM49574     1  0.2632      0.867 0.832 0.000 0.000 0.004 0.000 NA
#&gt; GSM49575     1  0.1732      0.878 0.920 0.000 0.000 0.004 0.004 NA
#&gt; GSM49576     5  0.1921      0.875 0.032 0.000 0.000 0.000 0.916 NA
#&gt; GSM49577     5  0.4475      0.572 0.032 0.000 0.000 0.000 0.556 NA
#&gt; GSM49578     1  0.0000      0.880 1.000 0.000 0.000 0.000 0.000 NA
#&gt; GSM49579     5  0.3062      0.841 0.032 0.000 0.000 0.000 0.824 NA
#&gt; GSM49580     1  0.1913      0.856 0.908 0.000 0.000 0.000 0.012 NA
#&gt; GSM49581     1  0.1141      0.871 0.948 0.000 0.000 0.000 0.000 NA
#&gt; GSM49582     1  0.0000      0.880 1.000 0.000 0.000 0.000 0.000 NA
#&gt; GSM49583     2  0.2562      0.851 0.000 0.828 0.000 0.000 0.000 NA
#&gt; GSM49584     1  0.0937      0.873 0.960 0.000 0.000 0.000 0.000 NA
#&gt; GSM49585     1  0.3454      0.822 0.760 0.000 0.000 0.012 0.004 NA
#&gt; GSM49586     5  0.3821      0.820 0.024 0.000 0.000 0.020 0.768 NA
#&gt; GSM49587     1  0.2234      0.873 0.872 0.000 0.000 0.004 0.000 NA
#&gt; GSM49588     1  0.2810      0.863 0.832 0.000 0.000 0.008 0.004 NA
#&gt; GSM49589     5  0.3275      0.874 0.032 0.000 0.000 0.008 0.820 NA
#&gt; GSM49590     5  0.1498      0.879 0.032 0.000 0.000 0.000 0.940 NA
#&gt; GSM49591     1  0.2778      0.861 0.824 0.000 0.000 0.008 0.000 NA
#&gt; GSM49592     1  0.2302      0.873 0.872 0.000 0.000 0.008 0.000 NA
#&gt; GSM49593     5  0.1644      0.881 0.040 0.000 0.000 0.000 0.932 NA
#&gt; GSM49594     5  0.3947      0.841 0.036 0.000 0.000 0.008 0.744 NA
#&gt; GSM49595     5  0.3918      0.844 0.036 0.000 0.000 0.008 0.748 NA
#&gt; GSM49596     5  0.1713      0.878 0.044 0.000 0.000 0.000 0.928 NA
#&gt; GSM49597     4  0.3699      0.777 0.000 0.036 0.000 0.752 0.000 NA
#&gt; GSM49598     5  0.2078      0.881 0.040 0.000 0.000 0.004 0.912 NA
#&gt; GSM49599     1  0.2595      0.872 0.836 0.000 0.000 0.004 0.000 NA
#&gt; GSM49600     1  0.3552      0.761 0.800 0.000 0.000 0.000 0.116 NA
#&gt; GSM49601     5  0.3433      0.859 0.040 0.000 0.000 0.012 0.816 NA
#&gt; GSM49602     5  0.2833      0.873 0.040 0.000 0.000 0.008 0.864 NA
#&gt; GSM49603     5  0.2833      0.873 0.040 0.000 0.000 0.008 0.864 NA
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-5-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-get-signatures'>
<ul>
<li><a href='#tab-MAD-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-kmeans-signature_compare](figure_cola/MAD-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-kmeans-collect-classes](figure_cola/MAD-kmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n disease.state(p) cell.type(p) k
#> MAD:kmeans 54         5.97e-07     6.72e-04 2
#> MAD:kmeans 54         2.57e-07     8.46e-14 3
#> MAD:kmeans 53         1.61e-06     2.73e-12 4
#> MAD:kmeans 54         1.42e-07     4.23e-13 5
#> MAD:kmeans 54         1.42e-07     4.23e-13 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:skmeans






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "skmeans"]
# you can also extract it by
# res = res_list["MAD:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21168 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-skmeans-collect-plots](figure_cola/MAD-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-skmeans-select-partition-number](figure_cola/MAD-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.811           0.888       0.954         0.4698 0.535   0.535
#> 3 3 0.887           0.892       0.952         0.4374 0.720   0.509
#> 4 4 0.759           0.820       0.900         0.1102 0.882   0.660
#> 5 5 0.664           0.602       0.788         0.0638 0.991   0.964
#> 6 6 0.664           0.528       0.715         0.0409 0.948   0.793
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-get-classes'>
<ul>
<li><a href='#tab-MAD-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-skmeans-get-classes-1'>
<p><a id='tab-MAD-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM49613     1  0.0376      0.950 0.996 0.004
#&gt; GSM49604     2  0.0000      0.941 0.000 1.000
#&gt; GSM49605     2  0.0000      0.941 0.000 1.000
#&gt; GSM49606     2  0.0000      0.941 0.000 1.000
#&gt; GSM49607     2  0.0000      0.941 0.000 1.000
#&gt; GSM49608     2  0.0000      0.941 0.000 1.000
#&gt; GSM49609     2  0.0000      0.941 0.000 1.000
#&gt; GSM49610     2  0.0000      0.941 0.000 1.000
#&gt; GSM49611     2  0.0000      0.941 0.000 1.000
#&gt; GSM49612     2  0.0000      0.941 0.000 1.000
#&gt; GSM49614     2  0.8267      0.643 0.260 0.740
#&gt; GSM49615     1  0.0376      0.950 0.996 0.004
#&gt; GSM49616     1  0.4022      0.881 0.920 0.080
#&gt; GSM49617     2  0.9963      0.151 0.464 0.536
#&gt; GSM49564     1  0.0000      0.952 1.000 0.000
#&gt; GSM49565     1  0.9460      0.440 0.636 0.364
#&gt; GSM49566     1  0.0000      0.952 1.000 0.000
#&gt; GSM49567     1  0.0938      0.945 0.988 0.012
#&gt; GSM49568     1  0.0000      0.952 1.000 0.000
#&gt; GSM49569     1  0.0000      0.952 1.000 0.000
#&gt; GSM49570     2  0.0000      0.941 0.000 1.000
#&gt; GSM49571     2  0.0000      0.941 0.000 1.000
#&gt; GSM49572     1  0.0672      0.948 0.992 0.008
#&gt; GSM49573     2  0.0000      0.941 0.000 1.000
#&gt; GSM49574     1  0.6712      0.772 0.824 0.176
#&gt; GSM49575     1  0.9129      0.521 0.672 0.328
#&gt; GSM49576     1  0.0000      0.952 1.000 0.000
#&gt; GSM49577     2  0.2948      0.906 0.052 0.948
#&gt; GSM49578     1  0.0000      0.952 1.000 0.000
#&gt; GSM49579     1  0.0000      0.952 1.000 0.000
#&gt; GSM49580     1  0.0000      0.952 1.000 0.000
#&gt; GSM49581     1  0.0000      0.952 1.000 0.000
#&gt; GSM49582     1  0.0000      0.952 1.000 0.000
#&gt; GSM49583     2  0.0000      0.941 0.000 1.000
#&gt; GSM49584     1  0.0000      0.952 1.000 0.000
#&gt; GSM49585     1  0.0000      0.952 1.000 0.000
#&gt; GSM49586     2  0.5629      0.828 0.132 0.868
#&gt; GSM49587     1  0.0000      0.952 1.000 0.000
#&gt; GSM49588     1  0.0000      0.952 1.000 0.000
#&gt; GSM49589     1  0.0376      0.950 0.996 0.004
#&gt; GSM49590     1  0.0376      0.950 0.996 0.004
#&gt; GSM49591     1  0.0000      0.952 1.000 0.000
#&gt; GSM49592     1  0.0000      0.952 1.000 0.000
#&gt; GSM49593     1  0.0000      0.952 1.000 0.000
#&gt; GSM49594     2  0.4298      0.875 0.088 0.912
#&gt; GSM49595     1  0.9775      0.303 0.588 0.412
#&gt; GSM49596     1  0.0000      0.952 1.000 0.000
#&gt; GSM49597     2  0.0000      0.941 0.000 1.000
#&gt; GSM49598     1  0.0000      0.952 1.000 0.000
#&gt; GSM49599     1  0.4815      0.862 0.896 0.104
#&gt; GSM49600     1  0.0000      0.952 1.000 0.000
#&gt; GSM49601     1  0.0000      0.952 1.000 0.000
#&gt; GSM49602     1  0.0000      0.952 1.000 0.000
#&gt; GSM49603     1  0.0000      0.952 1.000 0.000
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-1-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-2'>
<p><a id='tab-MAD-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM49613     3  0.0000      0.940 0.000 0.000 1.000
#&gt; GSM49604     2  0.0000      0.945 0.000 1.000 0.000
#&gt; GSM49605     2  0.0000      0.945 0.000 1.000 0.000
#&gt; GSM49606     2  0.0000      0.945 0.000 1.000 0.000
#&gt; GSM49607     2  0.0000      0.945 0.000 1.000 0.000
#&gt; GSM49608     2  0.0000      0.945 0.000 1.000 0.000
#&gt; GSM49609     2  0.0000      0.945 0.000 1.000 0.000
#&gt; GSM49610     2  0.0000      0.945 0.000 1.000 0.000
#&gt; GSM49611     2  0.0000      0.945 0.000 1.000 0.000
#&gt; GSM49612     2  0.0000      0.945 0.000 1.000 0.000
#&gt; GSM49614     3  0.2796      0.872 0.000 0.092 0.908
#&gt; GSM49615     3  0.0000      0.940 0.000 0.000 1.000
#&gt; GSM49616     3  0.0000      0.940 0.000 0.000 1.000
#&gt; GSM49617     3  0.0000      0.940 0.000 0.000 1.000
#&gt; GSM49564     3  0.0000      0.940 0.000 0.000 1.000
#&gt; GSM49565     1  0.0237      0.957 0.996 0.004 0.000
#&gt; GSM49566     3  0.2448      0.897 0.076 0.000 0.924
#&gt; GSM49567     1  0.0237      0.958 0.996 0.000 0.004
#&gt; GSM49568     1  0.0424      0.957 0.992 0.000 0.008
#&gt; GSM49569     3  0.0592      0.938 0.012 0.000 0.988
#&gt; GSM49570     2  0.0000      0.945 0.000 1.000 0.000
#&gt; GSM49571     2  0.0661      0.937 0.008 0.988 0.004
#&gt; GSM49572     1  0.0237      0.958 0.996 0.000 0.004
#&gt; GSM49573     2  0.0000      0.945 0.000 1.000 0.000
#&gt; GSM49574     1  0.0000      0.959 1.000 0.000 0.000
#&gt; GSM49575     1  0.0475      0.957 0.992 0.004 0.004
#&gt; GSM49576     3  0.0237      0.939 0.004 0.000 0.996
#&gt; GSM49577     2  0.7337      0.506 0.300 0.644 0.056
#&gt; GSM49578     1  0.0000      0.959 1.000 0.000 0.000
#&gt; GSM49579     3  0.4121      0.797 0.168 0.000 0.832
#&gt; GSM49580     1  0.1031      0.947 0.976 0.000 0.024
#&gt; GSM49581     1  0.0000      0.959 1.000 0.000 0.000
#&gt; GSM49582     1  0.0000      0.959 1.000 0.000 0.000
#&gt; GSM49583     2  0.0000      0.945 0.000 1.000 0.000
#&gt; GSM49584     1  0.0237      0.958 0.996 0.000 0.004
#&gt; GSM49585     1  0.1031      0.947 0.976 0.000 0.024
#&gt; GSM49586     3  0.6769      0.324 0.016 0.392 0.592
#&gt; GSM49587     1  0.0000      0.959 1.000 0.000 0.000
#&gt; GSM49588     1  0.1163      0.943 0.972 0.000 0.028
#&gt; GSM49589     3  0.0000      0.940 0.000 0.000 1.000
#&gt; GSM49590     3  0.0000      0.940 0.000 0.000 1.000
#&gt; GSM49591     1  0.0000      0.959 1.000 0.000 0.000
#&gt; GSM49592     1  0.0000      0.959 1.000 0.000 0.000
#&gt; GSM49593     3  0.0892      0.937 0.020 0.000 0.980
#&gt; GSM49594     2  0.7192      0.203 0.028 0.560 0.412
#&gt; GSM49595     3  0.5060      0.833 0.064 0.100 0.836
#&gt; GSM49596     1  0.5859      0.476 0.656 0.000 0.344
#&gt; GSM49597     2  0.0000      0.945 0.000 1.000 0.000
#&gt; GSM49598     3  0.1964      0.919 0.056 0.000 0.944
#&gt; GSM49599     1  0.0424      0.955 0.992 0.008 0.000
#&gt; GSM49600     1  0.5138      0.677 0.748 0.000 0.252
#&gt; GSM49601     3  0.1643      0.925 0.044 0.000 0.956
#&gt; GSM49602     3  0.0592      0.938 0.012 0.000 0.988
#&gt; GSM49603     3  0.0424      0.939 0.008 0.000 0.992
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-2-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-3'>
<p><a id='tab-MAD-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM49613     3  0.0188      0.892 0.000 0.000 0.996 0.004
#&gt; GSM49604     2  0.0000      0.969 0.000 1.000 0.000 0.000
#&gt; GSM49605     2  0.0000      0.969 0.000 1.000 0.000 0.000
#&gt; GSM49606     2  0.0000      0.969 0.000 1.000 0.000 0.000
#&gt; GSM49607     2  0.0000      0.969 0.000 1.000 0.000 0.000
#&gt; GSM49608     2  0.0000      0.969 0.000 1.000 0.000 0.000
#&gt; GSM49609     2  0.0000      0.969 0.000 1.000 0.000 0.000
#&gt; GSM49610     2  0.0000      0.969 0.000 1.000 0.000 0.000
#&gt; GSM49611     2  0.0000      0.969 0.000 1.000 0.000 0.000
#&gt; GSM49612     2  0.0000      0.969 0.000 1.000 0.000 0.000
#&gt; GSM49614     3  0.0707      0.875 0.000 0.020 0.980 0.000
#&gt; GSM49615     3  0.0188      0.892 0.000 0.000 0.996 0.004
#&gt; GSM49616     3  0.0188      0.892 0.000 0.000 0.996 0.004
#&gt; GSM49617     3  0.0000      0.891 0.000 0.000 1.000 0.000
#&gt; GSM49564     3  0.2053      0.852 0.004 0.000 0.924 0.072
#&gt; GSM49565     1  0.2960      0.887 0.892 0.020 0.004 0.084
#&gt; GSM49566     4  0.6762      0.374 0.104 0.000 0.360 0.536
#&gt; GSM49567     1  0.0817      0.916 0.976 0.000 0.000 0.024
#&gt; GSM49568     1  0.2131      0.908 0.932 0.000 0.032 0.036
#&gt; GSM49569     4  0.5508      0.627 0.056 0.000 0.252 0.692
#&gt; GSM49570     2  0.2081      0.902 0.000 0.916 0.084 0.000
#&gt; GSM49571     2  0.5496      0.646 0.016 0.724 0.040 0.220
#&gt; GSM49572     1  0.0336      0.915 0.992 0.000 0.000 0.008
#&gt; GSM49573     2  0.0188      0.967 0.000 0.996 0.004 0.000
#&gt; GSM49574     1  0.0817      0.916 0.976 0.000 0.000 0.024
#&gt; GSM49575     1  0.2465      0.906 0.924 0.012 0.020 0.044
#&gt; GSM49576     4  0.4830      0.394 0.000 0.000 0.392 0.608
#&gt; GSM49577     4  0.8104      0.414 0.176 0.276 0.036 0.512
#&gt; GSM49578     1  0.0000      0.913 1.000 0.000 0.000 0.000
#&gt; GSM49579     4  0.4938      0.701 0.080 0.000 0.148 0.772
#&gt; GSM49580     1  0.2522      0.885 0.908 0.000 0.076 0.016
#&gt; GSM49581     1  0.1302      0.914 0.956 0.000 0.000 0.044
#&gt; GSM49582     1  0.0188      0.914 0.996 0.000 0.000 0.004
#&gt; GSM49583     2  0.0000      0.969 0.000 1.000 0.000 0.000
#&gt; GSM49584     1  0.0779      0.914 0.980 0.000 0.016 0.004
#&gt; GSM49585     1  0.5102      0.739 0.732 0.000 0.048 0.220
#&gt; GSM49586     4  0.1520      0.779 0.000 0.024 0.020 0.956
#&gt; GSM49587     1  0.0895      0.917 0.976 0.000 0.004 0.020
#&gt; GSM49588     1  0.4434      0.738 0.756 0.000 0.016 0.228
#&gt; GSM49589     3  0.2760      0.806 0.000 0.000 0.872 0.128
#&gt; GSM49590     3  0.4843      0.205 0.000 0.000 0.604 0.396
#&gt; GSM49591     1  0.2408      0.888 0.896 0.000 0.000 0.104
#&gt; GSM49592     1  0.0817      0.916 0.976 0.000 0.000 0.024
#&gt; GSM49593     4  0.2546      0.775 0.008 0.000 0.092 0.900
#&gt; GSM49594     4  0.3432      0.717 0.008 0.120 0.012 0.860
#&gt; GSM49595     4  0.0524      0.779 0.000 0.004 0.008 0.988
#&gt; GSM49596     4  0.5972      0.529 0.292 0.000 0.068 0.640
#&gt; GSM49597     2  0.1302      0.937 0.000 0.956 0.044 0.000
#&gt; GSM49598     4  0.2319      0.783 0.036 0.000 0.040 0.924
#&gt; GSM49599     1  0.2895      0.895 0.908 0.032 0.016 0.044
#&gt; GSM49600     1  0.6457      0.555 0.644 0.000 0.200 0.156
#&gt; GSM49601     4  0.1022      0.781 0.000 0.000 0.032 0.968
#&gt; GSM49602     4  0.1474      0.782 0.000 0.000 0.052 0.948
#&gt; GSM49603     4  0.1867      0.780 0.000 0.000 0.072 0.928
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-3-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-4'>
<p><a id='tab-MAD-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM49613     3  0.0000     0.8353 0.000 0.000 1.000 0.000 0.000
#&gt; GSM49604     2  0.1410     0.8593 0.000 0.940 0.000 0.060 0.000
#&gt; GSM49605     2  0.0000     0.8843 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49606     2  0.0000     0.8843 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49607     2  0.0000     0.8843 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49608     2  0.0162     0.8839 0.000 0.996 0.000 0.004 0.000
#&gt; GSM49609     2  0.0290     0.8836 0.000 0.992 0.000 0.008 0.000
#&gt; GSM49610     2  0.0000     0.8843 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49611     2  0.0290     0.8836 0.000 0.992 0.000 0.008 0.000
#&gt; GSM49612     2  0.0290     0.8836 0.000 0.992 0.000 0.008 0.000
#&gt; GSM49614     3  0.0162     0.8323 0.000 0.000 0.996 0.004 0.000
#&gt; GSM49615     3  0.0000     0.8353 0.000 0.000 1.000 0.000 0.000
#&gt; GSM49616     3  0.0000     0.8353 0.000 0.000 1.000 0.000 0.000
#&gt; GSM49617     3  0.0000     0.8353 0.000 0.000 1.000 0.000 0.000
#&gt; GSM49564     3  0.4743     0.6695 0.040 0.000 0.772 0.064 0.124
#&gt; GSM49565     1  0.5371     0.5588 0.596 0.020 0.000 0.352 0.032
#&gt; GSM49566     5  0.8138    -0.0539 0.128 0.000 0.192 0.304 0.376
#&gt; GSM49567     1  0.4622     0.6260 0.696 0.000 0.008 0.268 0.028
#&gt; GSM49568     1  0.3543     0.7125 0.828 0.000 0.012 0.136 0.024
#&gt; GSM49569     5  0.6180     0.3964 0.040 0.000 0.168 0.148 0.644
#&gt; GSM49570     2  0.5136     0.6242 0.000 0.692 0.128 0.180 0.000
#&gt; GSM49571     2  0.7504    -0.0119 0.044 0.452 0.008 0.316 0.180
#&gt; GSM49572     1  0.4161     0.6598 0.704 0.000 0.000 0.280 0.016
#&gt; GSM49573     2  0.4251     0.6889 0.004 0.740 0.020 0.232 0.004
#&gt; GSM49574     1  0.3527     0.7046 0.792 0.000 0.000 0.192 0.016
#&gt; GSM49575     1  0.5217     0.5895 0.648 0.004 0.008 0.296 0.044
#&gt; GSM49576     5  0.6127     0.2385 0.000 0.000 0.316 0.152 0.532
#&gt; GSM49577     4  0.8766     0.0000 0.112 0.180 0.040 0.372 0.296
#&gt; GSM49578     1  0.0880     0.7217 0.968 0.000 0.000 0.032 0.000
#&gt; GSM49579     5  0.6858     0.1018 0.088 0.000 0.064 0.348 0.500
#&gt; GSM49580     1  0.5322     0.6188 0.716 0.000 0.040 0.176 0.068
#&gt; GSM49581     1  0.4088     0.6723 0.776 0.000 0.000 0.168 0.056
#&gt; GSM49582     1  0.0880     0.7206 0.968 0.000 0.000 0.032 0.000
#&gt; GSM49583     2  0.0510     0.8810 0.000 0.984 0.000 0.016 0.000
#&gt; GSM49584     1  0.2777     0.7164 0.864 0.000 0.016 0.120 0.000
#&gt; GSM49585     1  0.6674     0.4832 0.544 0.000 0.032 0.284 0.140
#&gt; GSM49586     5  0.4225     0.4951 0.012 0.008 0.016 0.196 0.768
#&gt; GSM49587     1  0.2930     0.7158 0.832 0.000 0.000 0.164 0.004
#&gt; GSM49588     1  0.6311     0.4981 0.568 0.000 0.012 0.264 0.156
#&gt; GSM49589     3  0.4432     0.6758 0.008 0.000 0.772 0.076 0.144
#&gt; GSM49590     3  0.5452    -0.0585 0.000 0.000 0.492 0.060 0.448
#&gt; GSM49591     1  0.4221     0.6661 0.732 0.000 0.000 0.236 0.032
#&gt; GSM49592     1  0.2411     0.7181 0.884 0.000 0.000 0.108 0.008
#&gt; GSM49593     5  0.2151     0.5934 0.016 0.000 0.020 0.040 0.924
#&gt; GSM49594     5  0.5781     0.3122 0.004 0.116 0.012 0.212 0.656
#&gt; GSM49595     5  0.3740     0.5266 0.008 0.016 0.004 0.168 0.804
#&gt; GSM49596     5  0.6960     0.1454 0.244 0.000 0.040 0.184 0.532
#&gt; GSM49597     2  0.3112     0.8014 0.000 0.856 0.044 0.100 0.000
#&gt; GSM49598     5  0.3736     0.5674 0.052 0.000 0.020 0.092 0.836
#&gt; GSM49599     1  0.6045     0.4863 0.552 0.020 0.012 0.368 0.048
#&gt; GSM49600     1  0.7744     0.1860 0.460 0.000 0.092 0.236 0.212
#&gt; GSM49601     5  0.2635     0.5846 0.016 0.000 0.008 0.088 0.888
#&gt; GSM49602     5  0.1701     0.5964 0.000 0.000 0.016 0.048 0.936
#&gt; GSM49603     5  0.1560     0.6000 0.004 0.000 0.028 0.020 0.948
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-4-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-5'>
<p><a id='tab-MAD-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM49613     3  0.0000     0.8749 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49604     2  0.3073     0.5463 0.000 0.788 0.000 0.204 0.000 0.008
#&gt; GSM49605     2  0.0692     0.8936 0.000 0.976 0.000 0.020 0.000 0.004
#&gt; GSM49606     2  0.0405     0.8987 0.000 0.988 0.000 0.004 0.000 0.008
#&gt; GSM49607     2  0.0622     0.8966 0.000 0.980 0.000 0.012 0.000 0.008
#&gt; GSM49608     2  0.0405     0.8983 0.000 0.988 0.000 0.004 0.000 0.008
#&gt; GSM49609     2  0.0260     0.9003 0.000 0.992 0.000 0.008 0.000 0.000
#&gt; GSM49610     2  0.0260     0.9003 0.000 0.992 0.000 0.008 0.000 0.000
#&gt; GSM49611     2  0.0260     0.9003 0.000 0.992 0.000 0.008 0.000 0.000
#&gt; GSM49612     2  0.0146     0.8994 0.000 0.996 0.000 0.004 0.000 0.000
#&gt; GSM49614     3  0.0458     0.8661 0.000 0.000 0.984 0.016 0.000 0.000
#&gt; GSM49615     3  0.0000     0.8749 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49616     3  0.0000     0.8749 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49617     3  0.0146     0.8731 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; GSM49564     3  0.5234     0.6217 0.060 0.000 0.724 0.024 0.116 0.076
#&gt; GSM49565     1  0.6818     0.3899 0.480 0.044 0.000 0.160 0.020 0.296
#&gt; GSM49566     6  0.7887     0.2781 0.116 0.004 0.088 0.080 0.296 0.416
#&gt; GSM49567     1  0.5266     0.4469 0.580 0.000 0.000 0.096 0.008 0.316
#&gt; GSM49568     1  0.4254     0.5941 0.788 0.000 0.012 0.056 0.036 0.108
#&gt; GSM49569     5  0.6685     0.2377 0.056 0.000 0.080 0.076 0.584 0.204
#&gt; GSM49570     4  0.5506     0.4945 0.000 0.452 0.084 0.452 0.004 0.008
#&gt; GSM49571     4  0.6650     0.4102 0.008 0.220 0.016 0.556 0.156 0.044
#&gt; GSM49572     1  0.5272     0.5104 0.600 0.000 0.000 0.128 0.004 0.268
#&gt; GSM49573     4  0.4305     0.5872 0.004 0.424 0.008 0.560 0.004 0.000
#&gt; GSM49574     1  0.5418     0.5716 0.648 0.000 0.000 0.168 0.028 0.156
#&gt; GSM49575     1  0.6366     0.4063 0.504 0.008 0.004 0.328 0.032 0.124
#&gt; GSM49576     5  0.6844    -0.0134 0.008 0.000 0.276 0.036 0.424 0.256
#&gt; GSM49577     6  0.8288     0.3181 0.112 0.128 0.004 0.180 0.148 0.428
#&gt; GSM49578     1  0.1890     0.6171 0.916 0.000 0.000 0.024 0.000 0.060
#&gt; GSM49579     6  0.6084     0.2474 0.044 0.000 0.040 0.040 0.328 0.548
#&gt; GSM49580     1  0.6700     0.4389 0.572 0.000 0.036 0.112 0.072 0.208
#&gt; GSM49581     1  0.5250     0.5474 0.692 0.000 0.008 0.148 0.032 0.120
#&gt; GSM49582     1  0.1794     0.6127 0.924 0.000 0.000 0.040 0.000 0.036
#&gt; GSM49583     2  0.1584     0.8498 0.000 0.928 0.000 0.064 0.000 0.008
#&gt; GSM49584     1  0.3812     0.5996 0.812 0.000 0.024 0.068 0.004 0.092
#&gt; GSM49585     1  0.8116     0.1724 0.352 0.000 0.036 0.208 0.168 0.236
#&gt; GSM49586     5  0.4841     0.3072 0.004 0.004 0.000 0.292 0.636 0.064
#&gt; GSM49587     1  0.4466     0.5917 0.716 0.000 0.000 0.100 0.004 0.180
#&gt; GSM49588     1  0.6833     0.4478 0.524 0.000 0.008 0.160 0.096 0.212
#&gt; GSM49589     3  0.6235     0.4335 0.008 0.000 0.604 0.084 0.184 0.120
#&gt; GSM49590     5  0.5781     0.0547 0.000 0.000 0.412 0.008 0.444 0.136
#&gt; GSM49591     1  0.6092     0.5033 0.572 0.000 0.000 0.160 0.048 0.220
#&gt; GSM49592     1  0.4004     0.5968 0.780 0.000 0.000 0.100 0.012 0.108
#&gt; GSM49593     5  0.3748     0.4599 0.016 0.000 0.020 0.052 0.824 0.088
#&gt; GSM49594     5  0.7103     0.1369 0.008 0.120 0.008 0.128 0.512 0.224
#&gt; GSM49595     5  0.5164     0.3103 0.008 0.004 0.000 0.108 0.644 0.236
#&gt; GSM49596     5  0.7383    -0.0884 0.208 0.000 0.024 0.080 0.444 0.244
#&gt; GSM49597     2  0.4582     0.4102 0.000 0.716 0.040 0.204 0.000 0.040
#&gt; GSM49598     5  0.5281     0.3968 0.040 0.000 0.032 0.068 0.712 0.148
#&gt; GSM49599     1  0.7008     0.2989 0.388 0.024 0.004 0.364 0.024 0.196
#&gt; GSM49600     1  0.8061     0.1375 0.424 0.000 0.112 0.096 0.136 0.232
#&gt; GSM49601     5  0.2866     0.4730 0.012 0.000 0.000 0.060 0.868 0.060
#&gt; GSM49602     5  0.1528     0.4865 0.000 0.000 0.000 0.016 0.936 0.048
#&gt; GSM49603     5  0.1528     0.4920 0.000 0.000 0.012 0.016 0.944 0.028
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-5-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-get-signatures'>
<ul>
<li><a href='#tab-MAD-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-skmeans-signature_compare](figure_cola/MAD-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-skmeans-collect-classes](figure_cola/MAD-skmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>              n disease.state(p) cell.type(p) k
#> MAD:skmeans 51         2.44e-04     1.83e-02 2
#> MAD:skmeans 51         1.53e-05     5.42e-04 3
#> MAD:skmeans 50         1.91e-05     5.19e-08 4
#> MAD:skmeans 40         1.24e-04     3.51e-06 5
#> MAD:skmeans 27         2.09e-04     7.73e-05 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:pam*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "pam"]
# you can also extract it by
# res = res_list["MAD:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21168 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 5.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-pam-collect-plots](figure_cola/MAD-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-pam-select-partition-number](figure_cola/MAD-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.963       0.987         0.3379 0.669   0.669
#> 3 3 0.671           0.826       0.874         0.5489 0.834   0.754
#> 4 4 0.820           0.831       0.933         0.3548 0.741   0.513
#> 5 5 0.944           0.891       0.957         0.0429 0.941   0.806
#> 6 6 0.853           0.841       0.911         0.0324 0.983   0.936
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 5
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-get-classes'>
<ul>
<li><a href='#tab-MAD-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-pam-get-classes-1'>
<p><a id='tab-MAD-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM49613     1  0.0000      0.987 1.000 0.000
#&gt; GSM49604     2  0.0376      0.977 0.004 0.996
#&gt; GSM49605     2  0.0000      0.980 0.000 1.000
#&gt; GSM49606     2  0.0000      0.980 0.000 1.000
#&gt; GSM49607     2  0.0000      0.980 0.000 1.000
#&gt; GSM49608     2  0.0000      0.980 0.000 1.000
#&gt; GSM49609     2  0.0000      0.980 0.000 1.000
#&gt; GSM49610     2  0.0000      0.980 0.000 1.000
#&gt; GSM49611     2  0.0000      0.980 0.000 1.000
#&gt; GSM49612     2  0.0000      0.980 0.000 1.000
#&gt; GSM49614     1  0.0000      0.987 1.000 0.000
#&gt; GSM49615     1  0.0000      0.987 1.000 0.000
#&gt; GSM49616     1  0.0000      0.987 1.000 0.000
#&gt; GSM49617     1  0.0000      0.987 1.000 0.000
#&gt; GSM49564     1  0.0000      0.987 1.000 0.000
#&gt; GSM49565     1  0.0000      0.987 1.000 0.000
#&gt; GSM49566     1  0.0000      0.987 1.000 0.000
#&gt; GSM49567     1  0.0000      0.987 1.000 0.000
#&gt; GSM49568     1  0.0000      0.987 1.000 0.000
#&gt; GSM49569     1  0.0000      0.987 1.000 0.000
#&gt; GSM49570     1  1.0000     -0.025 0.504 0.496
#&gt; GSM49571     1  0.0000      0.987 1.000 0.000
#&gt; GSM49572     1  0.0000      0.987 1.000 0.000
#&gt; GSM49573     1  0.1414      0.967 0.980 0.020
#&gt; GSM49574     1  0.0000      0.987 1.000 0.000
#&gt; GSM49575     1  0.0000      0.987 1.000 0.000
#&gt; GSM49576     1  0.0000      0.987 1.000 0.000
#&gt; GSM49577     1  0.0000      0.987 1.000 0.000
#&gt; GSM49578     1  0.0000      0.987 1.000 0.000
#&gt; GSM49579     1  0.0000      0.987 1.000 0.000
#&gt; GSM49580     1  0.0000      0.987 1.000 0.000
#&gt; GSM49581     1  0.0000      0.987 1.000 0.000
#&gt; GSM49582     1  0.0000      0.987 1.000 0.000
#&gt; GSM49583     2  0.0000      0.980 0.000 1.000
#&gt; GSM49584     1  0.0000      0.987 1.000 0.000
#&gt; GSM49585     1  0.0000      0.987 1.000 0.000
#&gt; GSM49586     1  0.0000      0.987 1.000 0.000
#&gt; GSM49587     1  0.0000      0.987 1.000 0.000
#&gt; GSM49588     1  0.0000      0.987 1.000 0.000
#&gt; GSM49589     1  0.0000      0.987 1.000 0.000
#&gt; GSM49590     1  0.0000      0.987 1.000 0.000
#&gt; GSM49591     1  0.0000      0.987 1.000 0.000
#&gt; GSM49592     1  0.0000      0.987 1.000 0.000
#&gt; GSM49593     1  0.0000      0.987 1.000 0.000
#&gt; GSM49594     1  0.0000      0.987 1.000 0.000
#&gt; GSM49595     1  0.0000      0.987 1.000 0.000
#&gt; GSM49596     1  0.0000      0.987 1.000 0.000
#&gt; GSM49597     2  0.6973      0.763 0.188 0.812
#&gt; GSM49598     1  0.0000      0.987 1.000 0.000
#&gt; GSM49599     1  0.0000      0.987 1.000 0.000
#&gt; GSM49600     1  0.0000      0.987 1.000 0.000
#&gt; GSM49601     1  0.0000      0.987 1.000 0.000
#&gt; GSM49602     1  0.0000      0.987 1.000 0.000
#&gt; GSM49603     1  0.0000      0.987 1.000 0.000
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-1-a').click(function(){
  $('#tab-MAD-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-2'>
<p><a id='tab-MAD-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM49613     3  0.0000      0.985 0.000 0.000 1.000
#&gt; GSM49604     2  0.0000      0.919 0.000 1.000 0.000
#&gt; GSM49605     2  0.0000      0.919 0.000 1.000 0.000
#&gt; GSM49606     2  0.0000      0.919 0.000 1.000 0.000
#&gt; GSM49607     2  0.0000      0.919 0.000 1.000 0.000
#&gt; GSM49608     2  0.0000      0.919 0.000 1.000 0.000
#&gt; GSM49609     2  0.0000      0.919 0.000 1.000 0.000
#&gt; GSM49610     2  0.0000      0.919 0.000 1.000 0.000
#&gt; GSM49611     2  0.0000      0.919 0.000 1.000 0.000
#&gt; GSM49612     2  0.0000      0.919 0.000 1.000 0.000
#&gt; GSM49614     3  0.0424      0.984 0.008 0.000 0.992
#&gt; GSM49615     3  0.0424      0.985 0.008 0.000 0.992
#&gt; GSM49616     3  0.0000      0.985 0.000 0.000 1.000
#&gt; GSM49617     3  0.0892      0.973 0.020 0.000 0.980
#&gt; GSM49564     1  0.5529      0.816 0.704 0.000 0.296
#&gt; GSM49565     1  0.0000      0.791 1.000 0.000 0.000
#&gt; GSM49566     1  0.5254      0.830 0.736 0.000 0.264
#&gt; GSM49567     1  0.0000      0.791 1.000 0.000 0.000
#&gt; GSM49568     1  0.0000      0.791 1.000 0.000 0.000
#&gt; GSM49569     1  0.5431      0.826 0.716 0.000 0.284
#&gt; GSM49570     2  0.8195     -0.099 0.072 0.492 0.436
#&gt; GSM49571     1  0.5431      0.826 0.716 0.000 0.284
#&gt; GSM49572     1  0.0000      0.791 1.000 0.000 0.000
#&gt; GSM49573     1  0.7770      0.730 0.640 0.088 0.272
#&gt; GSM49574     1  0.0000      0.791 1.000 0.000 0.000
#&gt; GSM49575     1  0.0237      0.792 0.996 0.000 0.004
#&gt; GSM49576     1  0.5363      0.827 0.724 0.000 0.276
#&gt; GSM49577     1  0.5397      0.827 0.720 0.000 0.280
#&gt; GSM49578     1  0.0000      0.791 1.000 0.000 0.000
#&gt; GSM49579     1  0.5254      0.828 0.736 0.000 0.264
#&gt; GSM49580     1  0.1753      0.804 0.952 0.000 0.048
#&gt; GSM49581     1  0.0000      0.791 1.000 0.000 0.000
#&gt; GSM49582     1  0.0000      0.791 1.000 0.000 0.000
#&gt; GSM49583     2  0.0000      0.919 0.000 1.000 0.000
#&gt; GSM49584     1  0.0000      0.791 1.000 0.000 0.000
#&gt; GSM49585     1  0.2448      0.809 0.924 0.000 0.076
#&gt; GSM49586     1  0.5431      0.826 0.716 0.000 0.284
#&gt; GSM49587     1  0.0000      0.791 1.000 0.000 0.000
#&gt; GSM49588     1  0.5216      0.829 0.740 0.000 0.260
#&gt; GSM49589     1  0.5431      0.826 0.716 0.000 0.284
#&gt; GSM49590     1  0.5431      0.826 0.716 0.000 0.284
#&gt; GSM49591     1  0.0424      0.793 0.992 0.000 0.008
#&gt; GSM49592     1  0.0237      0.792 0.996 0.000 0.004
#&gt; GSM49593     1  0.5431      0.826 0.716 0.000 0.284
#&gt; GSM49594     1  0.5431      0.826 0.716 0.000 0.284
#&gt; GSM49595     1  0.5431      0.826 0.716 0.000 0.284
#&gt; GSM49596     1  0.5138      0.830 0.748 0.000 0.252
#&gt; GSM49597     2  0.4700      0.642 0.008 0.812 0.180
#&gt; GSM49598     1  0.5431      0.826 0.716 0.000 0.284
#&gt; GSM49599     1  0.4702      0.826 0.788 0.000 0.212
#&gt; GSM49600     1  0.1289      0.801 0.968 0.000 0.032
#&gt; GSM49601     1  0.5431      0.826 0.716 0.000 0.284
#&gt; GSM49602     1  0.5431      0.826 0.716 0.000 0.284
#&gt; GSM49603     1  0.5431      0.826 0.716 0.000 0.284
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-2-a').click(function(){
  $('#tab-MAD-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-3'>
<p><a id='tab-MAD-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM49613     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM49604     2  0.0000      0.972 0.000 1.000 0.000 0.000
#&gt; GSM49605     2  0.0000      0.972 0.000 1.000 0.000 0.000
#&gt; GSM49606     2  0.0000      0.972 0.000 1.000 0.000 0.000
#&gt; GSM49607     2  0.0000      0.972 0.000 1.000 0.000 0.000
#&gt; GSM49608     2  0.0000      0.972 0.000 1.000 0.000 0.000
#&gt; GSM49609     2  0.0000      0.972 0.000 1.000 0.000 0.000
#&gt; GSM49610     2  0.0000      0.972 0.000 1.000 0.000 0.000
#&gt; GSM49611     2  0.0000      0.972 0.000 1.000 0.000 0.000
#&gt; GSM49612     2  0.0000      0.972 0.000 1.000 0.000 0.000
#&gt; GSM49614     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM49615     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM49616     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM49617     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM49564     1  0.0000      0.912 1.000 0.000 0.000 0.000
#&gt; GSM49565     4  0.1389      0.836 0.048 0.000 0.000 0.952
#&gt; GSM49566     1  0.2647      0.825 0.880 0.000 0.000 0.120
#&gt; GSM49567     4  0.0000      0.844 0.000 0.000 0.000 1.000
#&gt; GSM49568     4  0.1389      0.836 0.048 0.000 0.000 0.952
#&gt; GSM49569     1  0.0000      0.912 1.000 0.000 0.000 0.000
#&gt; GSM49570     1  0.6494      0.171 0.532 0.400 0.004 0.064
#&gt; GSM49571     1  0.0000      0.912 1.000 0.000 0.000 0.000
#&gt; GSM49572     4  0.0000      0.844 0.000 0.000 0.000 1.000
#&gt; GSM49573     4  0.5850      0.103 0.456 0.032 0.000 0.512
#&gt; GSM49574     4  0.0592      0.844 0.016 0.000 0.000 0.984
#&gt; GSM49575     4  0.0469      0.842 0.012 0.000 0.000 0.988
#&gt; GSM49576     1  0.2011      0.867 0.920 0.000 0.000 0.080
#&gt; GSM49577     1  0.1474      0.890 0.948 0.000 0.000 0.052
#&gt; GSM49578     4  0.0000      0.844 0.000 0.000 0.000 1.000
#&gt; GSM49579     1  0.3123      0.789 0.844 0.000 0.000 0.156
#&gt; GSM49580     4  0.1474      0.827 0.052 0.000 0.000 0.948
#&gt; GSM49581     4  0.0000      0.844 0.000 0.000 0.000 1.000
#&gt; GSM49582     4  0.0000      0.844 0.000 0.000 0.000 1.000
#&gt; GSM49583     2  0.0000      0.972 0.000 1.000 0.000 0.000
#&gt; GSM49584     4  0.0000      0.844 0.000 0.000 0.000 1.000
#&gt; GSM49585     1  0.4877      0.219 0.592 0.000 0.000 0.408
#&gt; GSM49586     1  0.0000      0.912 1.000 0.000 0.000 0.000
#&gt; GSM49587     4  0.1211      0.839 0.040 0.000 0.000 0.960
#&gt; GSM49588     1  0.3172      0.785 0.840 0.000 0.000 0.160
#&gt; GSM49589     1  0.0000      0.912 1.000 0.000 0.000 0.000
#&gt; GSM49590     1  0.0000      0.912 1.000 0.000 0.000 0.000
#&gt; GSM49591     4  0.4933      0.238 0.432 0.000 0.000 0.568
#&gt; GSM49592     4  0.4761      0.418 0.372 0.000 0.000 0.628
#&gt; GSM49593     1  0.0000      0.912 1.000 0.000 0.000 0.000
#&gt; GSM49594     1  0.0592      0.907 0.984 0.000 0.000 0.016
#&gt; GSM49595     1  0.0000      0.912 1.000 0.000 0.000 0.000
#&gt; GSM49596     1  0.1389      0.887 0.952 0.000 0.000 0.048
#&gt; GSM49597     2  0.3528      0.703 0.192 0.808 0.000 0.000
#&gt; GSM49598     1  0.0707      0.905 0.980 0.000 0.000 0.020
#&gt; GSM49599     4  0.4522      0.491 0.320 0.000 0.000 0.680
#&gt; GSM49600     4  0.1557      0.828 0.056 0.000 0.000 0.944
#&gt; GSM49601     1  0.0000      0.912 1.000 0.000 0.000 0.000
#&gt; GSM49602     1  0.0000      0.912 1.000 0.000 0.000 0.000
#&gt; GSM49603     1  0.0000      0.912 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-3-a').click(function(){
  $('#tab-MAD-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-4'>
<p><a id='tab-MAD-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2 p3    p4    p5
#&gt; GSM49613     3  0.0000      1.000 0.000 0.000  1 0.000 0.000
#&gt; GSM49604     2  0.0880      0.967 0.000 0.968  0 0.032 0.000
#&gt; GSM49605     2  0.0000      0.996 0.000 1.000  0 0.000 0.000
#&gt; GSM49606     2  0.0000      0.996 0.000 1.000  0 0.000 0.000
#&gt; GSM49607     2  0.0000      0.996 0.000 1.000  0 0.000 0.000
#&gt; GSM49608     2  0.0000      0.996 0.000 1.000  0 0.000 0.000
#&gt; GSM49609     2  0.0000      0.996 0.000 1.000  0 0.000 0.000
#&gt; GSM49610     2  0.0000      0.996 0.000 1.000  0 0.000 0.000
#&gt; GSM49611     2  0.0000      0.996 0.000 1.000  0 0.000 0.000
#&gt; GSM49612     2  0.0000      0.996 0.000 1.000  0 0.000 0.000
#&gt; GSM49614     3  0.0000      1.000 0.000 0.000  1 0.000 0.000
#&gt; GSM49615     3  0.0000      1.000 0.000 0.000  1 0.000 0.000
#&gt; GSM49616     3  0.0000      1.000 0.000 0.000  1 0.000 0.000
#&gt; GSM49617     3  0.0000      1.000 0.000 0.000  1 0.000 0.000
#&gt; GSM49564     5  0.0000      0.921 0.000 0.000  0 0.000 1.000
#&gt; GSM49565     1  0.0566      0.906 0.984 0.000  0 0.004 0.012
#&gt; GSM49566     5  0.2516      0.807 0.140 0.000  0 0.000 0.860
#&gt; GSM49567     1  0.0000      0.913 1.000 0.000  0 0.000 0.000
#&gt; GSM49568     1  0.0162      0.912 0.996 0.000  0 0.004 0.000
#&gt; GSM49569     5  0.0000      0.921 0.000 0.000  0 0.000 1.000
#&gt; GSM49570     4  0.0162      0.965 0.000 0.000  0 0.996 0.004
#&gt; GSM49571     5  0.0880      0.905 0.000 0.000  0 0.032 0.968
#&gt; GSM49572     1  0.0162      0.912 0.996 0.000  0 0.004 0.000
#&gt; GSM49573     4  0.0162      0.964 0.004 0.000  0 0.996 0.000
#&gt; GSM49574     1  0.0162      0.912 0.996 0.000  0 0.004 0.000
#&gt; GSM49575     1  0.0290      0.909 0.992 0.000  0 0.000 0.008
#&gt; GSM49576     5  0.1282      0.905 0.044 0.000  0 0.004 0.952
#&gt; GSM49577     5  0.1043      0.909 0.040 0.000  0 0.000 0.960
#&gt; GSM49578     1  0.0000      0.913 1.000 0.000  0 0.000 0.000
#&gt; GSM49579     5  0.1831      0.882 0.076 0.000  0 0.004 0.920
#&gt; GSM49580     1  0.1270      0.872 0.948 0.000  0 0.000 0.052
#&gt; GSM49581     1  0.0000      0.913 1.000 0.000  0 0.000 0.000
#&gt; GSM49582     1  0.0000      0.913 1.000 0.000  0 0.000 0.000
#&gt; GSM49583     2  0.0000      0.996 0.000 1.000  0 0.000 0.000
#&gt; GSM49584     1  0.0000      0.913 1.000 0.000  0 0.000 0.000
#&gt; GSM49585     5  0.4118      0.493 0.336 0.000  0 0.004 0.660
#&gt; GSM49586     5  0.0162      0.920 0.000 0.000  0 0.004 0.996
#&gt; GSM49587     1  0.0162      0.912 0.996 0.000  0 0.004 0.000
#&gt; GSM49588     5  0.1892      0.879 0.080 0.000  0 0.004 0.916
#&gt; GSM49589     5  0.0000      0.921 0.000 0.000  0 0.000 1.000
#&gt; GSM49590     5  0.0000      0.921 0.000 0.000  0 0.000 1.000
#&gt; GSM49591     5  0.4448      0.107 0.480 0.000  0 0.004 0.516
#&gt; GSM49592     1  0.4367      0.211 0.580 0.000  0 0.004 0.416
#&gt; GSM49593     5  0.0000      0.921 0.000 0.000  0 0.000 1.000
#&gt; GSM49594     5  0.0451      0.919 0.008 0.000  0 0.004 0.988
#&gt; GSM49595     5  0.0000      0.921 0.000 0.000  0 0.000 1.000
#&gt; GSM49596     5  0.1043      0.904 0.040 0.000  0 0.000 0.960
#&gt; GSM49597     4  0.1579      0.936 0.000 0.024  0 0.944 0.032
#&gt; GSM49598     5  0.0609      0.916 0.020 0.000  0 0.000 0.980
#&gt; GSM49599     1  0.3857      0.497 0.688 0.000  0 0.000 0.312
#&gt; GSM49600     1  0.1197      0.878 0.952 0.000  0 0.000 0.048
#&gt; GSM49601     5  0.0000      0.921 0.000 0.000  0 0.000 1.000
#&gt; GSM49602     5  0.0000      0.921 0.000 0.000  0 0.000 1.000
#&gt; GSM49603     5  0.0000      0.921 0.000 0.000  0 0.000 1.000
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-4-a').click(function(){
  $('#tab-MAD-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-5'>
<p><a id='tab-MAD-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2 p3    p4    p5    p6
#&gt; GSM49613     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 0.000
#&gt; GSM49604     2  0.1575      0.881 0.000 0.936  0 0.032 0.000 0.032
#&gt; GSM49605     6  0.2597      0.868 0.000 0.176  0 0.000 0.000 0.824
#&gt; GSM49606     2  0.2793      0.633 0.000 0.800  0 0.000 0.000 0.200
#&gt; GSM49607     6  0.2597      0.868 0.000 0.176  0 0.000 0.000 0.824
#&gt; GSM49608     6  0.3023      0.845 0.000 0.232  0 0.000 0.000 0.768
#&gt; GSM49609     2  0.0000      0.928 0.000 1.000  0 0.000 0.000 0.000
#&gt; GSM49610     2  0.0000      0.928 0.000 1.000  0 0.000 0.000 0.000
#&gt; GSM49611     2  0.0000      0.928 0.000 1.000  0 0.000 0.000 0.000
#&gt; GSM49612     2  0.0000      0.928 0.000 1.000  0 0.000 0.000 0.000
#&gt; GSM49614     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 0.000
#&gt; GSM49615     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 0.000
#&gt; GSM49616     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 0.000
#&gt; GSM49617     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 0.000
#&gt; GSM49564     5  0.0363      0.893 0.000 0.000  0 0.000 0.988 0.012
#&gt; GSM49565     1  0.0909      0.896 0.968 0.000  0 0.000 0.012 0.020
#&gt; GSM49566     5  0.3149      0.786 0.132 0.000  0 0.000 0.824 0.044
#&gt; GSM49567     1  0.0937      0.890 0.960 0.000  0 0.000 0.000 0.040
#&gt; GSM49568     1  0.0000      0.901 1.000 0.000  0 0.000 0.000 0.000
#&gt; GSM49569     5  0.0260      0.893 0.000 0.000  0 0.000 0.992 0.008
#&gt; GSM49570     4  0.0146      0.858 0.000 0.000  0 0.996 0.000 0.004
#&gt; GSM49571     5  0.1789      0.880 0.000 0.000  0 0.032 0.924 0.044
#&gt; GSM49572     1  0.1204      0.885 0.944 0.000  0 0.000 0.000 0.056
#&gt; GSM49573     4  0.0146      0.857 0.000 0.000  0 0.996 0.000 0.004
#&gt; GSM49574     1  0.0000      0.901 1.000 0.000  0 0.000 0.000 0.000
#&gt; GSM49575     1  0.0405      0.899 0.988 0.000  0 0.000 0.008 0.004
#&gt; GSM49576     5  0.3240      0.835 0.040 0.000  0 0.000 0.812 0.148
#&gt; GSM49577     5  0.2826      0.853 0.028 0.000  0 0.000 0.844 0.128
#&gt; GSM49578     1  0.0000      0.901 1.000 0.000  0 0.000 0.000 0.000
#&gt; GSM49579     5  0.3680      0.813 0.072 0.000  0 0.000 0.784 0.144
#&gt; GSM49580     1  0.1398      0.871 0.940 0.000  0 0.000 0.052 0.008
#&gt; GSM49581     1  0.0146      0.900 0.996 0.000  0 0.000 0.000 0.004
#&gt; GSM49582     1  0.0146      0.900 0.996 0.000  0 0.000 0.000 0.004
#&gt; GSM49583     6  0.3747      0.584 0.000 0.396  0 0.000 0.000 0.604
#&gt; GSM49584     1  0.0458      0.899 0.984 0.000  0 0.000 0.000 0.016
#&gt; GSM49585     5  0.4552      0.552 0.288 0.000  0 0.000 0.648 0.064
#&gt; GSM49586     5  0.0405      0.892 0.000 0.000  0 0.004 0.988 0.008
#&gt; GSM49587     1  0.0937      0.890 0.960 0.000  0 0.000 0.000 0.040
#&gt; GSM49588     5  0.2554      0.849 0.076 0.000  0 0.000 0.876 0.048
#&gt; GSM49589     5  0.0260      0.892 0.000 0.000  0 0.000 0.992 0.008
#&gt; GSM49590     5  0.0937      0.886 0.000 0.000  0 0.000 0.960 0.040
#&gt; GSM49591     5  0.4636      0.195 0.444 0.000  0 0.000 0.516 0.040
#&gt; GSM49592     1  0.3782      0.195 0.588 0.000  0 0.000 0.412 0.000
#&gt; GSM49593     5  0.0260      0.892 0.000 0.000  0 0.000 0.992 0.008
#&gt; GSM49594     5  0.1806      0.876 0.004 0.000  0 0.000 0.908 0.088
#&gt; GSM49595     5  0.1501      0.879 0.000 0.000  0 0.000 0.924 0.076
#&gt; GSM49596     5  0.1723      0.883 0.036 0.000  0 0.000 0.928 0.036
#&gt; GSM49597     4  0.4239      0.666 0.000 0.016  0 0.696 0.024 0.264
#&gt; GSM49598     5  0.0806      0.891 0.020 0.000  0 0.000 0.972 0.008
#&gt; GSM49599     1  0.4445      0.496 0.656 0.000  0 0.000 0.288 0.056
#&gt; GSM49600     1  0.1333      0.878 0.944 0.000  0 0.000 0.048 0.008
#&gt; GSM49601     5  0.0260      0.892 0.000 0.000  0 0.000 0.992 0.008
#&gt; GSM49602     5  0.0260      0.893 0.000 0.000  0 0.000 0.992 0.008
#&gt; GSM49603     5  0.0260      0.893 0.000 0.000  0 0.000 0.992 0.008
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-5-a').click(function(){
  $('#tab-MAD-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-get-signatures'>
<ul>
<li><a href='#tab-MAD-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-1-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-1"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-2-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-2"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-3-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-3"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-4-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-4"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-5-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-pam-signature_compare](figure_cola/MAD-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-pam-collect-classes](figure_cola/MAD-pam-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>          n disease.state(p) cell.type(p) k
#> MAD:pam 53         2.67e-08     1.83e-04 2
#> MAD:pam 53         8.86e-09     3.34e-14 3
#> MAD:pam 48         2.88e-07     1.52e-11 4
#> MAD:pam 50         5.83e-08     1.88e-11 5
#> MAD:pam 51         8.68e-08     1.09e-13 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:mclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "mclust"]
# you can also extract it by
# res = res_list["MAD:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21168 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-mclust-collect-plots](figure_cola/MAD-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-mclust-select-partition-number](figure_cola/MAD-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.827           0.971       0.983          0.442 0.547   0.547
#> 3 3 1.000           0.973       0.988          0.128 0.955   0.917
#> 4 4 0.685           0.671       0.808          0.237 0.899   0.801
#> 5 5 0.796           0.746       0.871          0.114 0.800   0.542
#> 6 6 0.696           0.667       0.778          0.124 0.931   0.765
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-get-classes'>
<ul>
<li><a href='#tab-MAD-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-mclust-get-classes-1'>
<p><a id='tab-MAD-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM49613     2   0.595      0.873 0.144 0.856
#&gt; GSM49604     2   0.000      0.951 0.000 1.000
#&gt; GSM49605     2   0.000      0.951 0.000 1.000
#&gt; GSM49606     2   0.000      0.951 0.000 1.000
#&gt; GSM49607     2   0.000      0.951 0.000 1.000
#&gt; GSM49608     2   0.000      0.951 0.000 1.000
#&gt; GSM49609     2   0.000      0.951 0.000 1.000
#&gt; GSM49610     2   0.000      0.951 0.000 1.000
#&gt; GSM49611     2   0.000      0.951 0.000 1.000
#&gt; GSM49612     2   0.000      0.951 0.000 1.000
#&gt; GSM49614     2   0.595      0.873 0.144 0.856
#&gt; GSM49615     2   0.595      0.873 0.144 0.856
#&gt; GSM49616     2   0.595      0.873 0.144 0.856
#&gt; GSM49617     2   0.595      0.873 0.144 0.856
#&gt; GSM49564     1   0.000      0.996 1.000 0.000
#&gt; GSM49565     1   0.000      0.996 1.000 0.000
#&gt; GSM49566     1   0.000      0.996 1.000 0.000
#&gt; GSM49567     1   0.000      0.996 1.000 0.000
#&gt; GSM49568     1   0.000      0.996 1.000 0.000
#&gt; GSM49569     1   0.000      0.996 1.000 0.000
#&gt; GSM49570     2   0.000      0.951 0.000 1.000
#&gt; GSM49571     1   0.358      0.923 0.932 0.068
#&gt; GSM49572     1   0.000      0.996 1.000 0.000
#&gt; GSM49573     2   0.416      0.902 0.084 0.916
#&gt; GSM49574     1   0.000      0.996 1.000 0.000
#&gt; GSM49575     1   0.000      0.996 1.000 0.000
#&gt; GSM49576     1   0.000      0.996 1.000 0.000
#&gt; GSM49577     1   0.000      0.996 1.000 0.000
#&gt; GSM49578     1   0.000      0.996 1.000 0.000
#&gt; GSM49579     1   0.000      0.996 1.000 0.000
#&gt; GSM49580     1   0.000      0.996 1.000 0.000
#&gt; GSM49581     1   0.000      0.996 1.000 0.000
#&gt; GSM49582     1   0.000      0.996 1.000 0.000
#&gt; GSM49583     2   0.000      0.951 0.000 1.000
#&gt; GSM49584     1   0.000      0.996 1.000 0.000
#&gt; GSM49585     1   0.000      0.996 1.000 0.000
#&gt; GSM49586     1   0.000      0.996 1.000 0.000
#&gt; GSM49587     1   0.000      0.996 1.000 0.000
#&gt; GSM49588     1   0.000      0.996 1.000 0.000
#&gt; GSM49589     1   0.000      0.996 1.000 0.000
#&gt; GSM49590     1   0.295      0.942 0.948 0.052
#&gt; GSM49591     1   0.000      0.996 1.000 0.000
#&gt; GSM49592     1   0.000      0.996 1.000 0.000
#&gt; GSM49593     1   0.000      0.996 1.000 0.000
#&gt; GSM49594     1   0.000      0.996 1.000 0.000
#&gt; GSM49595     1   0.000      0.996 1.000 0.000
#&gt; GSM49596     1   0.000      0.996 1.000 0.000
#&gt; GSM49597     2   0.000      0.951 0.000 1.000
#&gt; GSM49598     1   0.000      0.996 1.000 0.000
#&gt; GSM49599     1   0.000      0.996 1.000 0.000
#&gt; GSM49600     1   0.000      0.996 1.000 0.000
#&gt; GSM49601     1   0.000      0.996 1.000 0.000
#&gt; GSM49602     1   0.000      0.996 1.000 0.000
#&gt; GSM49603     1   0.000      0.996 1.000 0.000
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-1-a').click(function(){
  $('#tab-MAD-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-2'>
<p><a id='tab-MAD-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM49613     3  0.0000      0.999 0.000 0.000 1.000
#&gt; GSM49604     2  0.1031      0.933 0.000 0.976 0.024
#&gt; GSM49605     2  0.0000      0.945 0.000 1.000 0.000
#&gt; GSM49606     2  0.0000      0.945 0.000 1.000 0.000
#&gt; GSM49607     2  0.0000      0.945 0.000 1.000 0.000
#&gt; GSM49608     2  0.0000      0.945 0.000 1.000 0.000
#&gt; GSM49609     2  0.0237      0.944 0.000 0.996 0.004
#&gt; GSM49610     2  0.0000      0.945 0.000 1.000 0.000
#&gt; GSM49611     2  0.0237      0.944 0.000 0.996 0.004
#&gt; GSM49612     2  0.0000      0.945 0.000 1.000 0.000
#&gt; GSM49614     3  0.0237      0.995 0.000 0.004 0.996
#&gt; GSM49615     3  0.0000      0.999 0.000 0.000 1.000
#&gt; GSM49616     3  0.0000      0.999 0.000 0.000 1.000
#&gt; GSM49617     3  0.0000      0.999 0.000 0.000 1.000
#&gt; GSM49564     1  0.0892      0.981 0.980 0.000 0.020
#&gt; GSM49565     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49566     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49567     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49568     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49569     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49570     2  0.4504      0.774 0.000 0.804 0.196
#&gt; GSM49571     1  0.0592      0.987 0.988 0.000 0.012
#&gt; GSM49572     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49573     2  0.5455      0.656 0.184 0.788 0.028
#&gt; GSM49574     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49575     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49576     1  0.0592      0.988 0.988 0.000 0.012
#&gt; GSM49577     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49578     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49579     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49580     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49581     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49582     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49583     2  0.0000      0.945 0.000 1.000 0.000
#&gt; GSM49584     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49585     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49586     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49587     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49588     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49589     1  0.0892      0.981 0.980 0.000 0.020
#&gt; GSM49590     1  0.1411      0.966 0.964 0.000 0.036
#&gt; GSM49591     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49592     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49593     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49594     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49595     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49596     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49597     2  0.3272      0.871 0.004 0.892 0.104
#&gt; GSM49598     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49599     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49600     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49601     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49602     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM49603     1  0.0000      0.997 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-2-a').click(function(){
  $('#tab-MAD-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-3'>
<p><a id='tab-MAD-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM49613     3  0.0000     1.0000 0.000 0.000 1.000 0.000
#&gt; GSM49604     2  0.1978     0.8561 0.000 0.928 0.004 0.068
#&gt; GSM49605     2  0.0000     0.8999 0.000 1.000 0.000 0.000
#&gt; GSM49606     2  0.0000     0.8999 0.000 1.000 0.000 0.000
#&gt; GSM49607     2  0.0000     0.8999 0.000 1.000 0.000 0.000
#&gt; GSM49608     2  0.0000     0.8999 0.000 1.000 0.000 0.000
#&gt; GSM49609     2  0.0000     0.8999 0.000 1.000 0.000 0.000
#&gt; GSM49610     2  0.0000     0.8999 0.000 1.000 0.000 0.000
#&gt; GSM49611     2  0.0000     0.8999 0.000 1.000 0.000 0.000
#&gt; GSM49612     2  0.0000     0.8999 0.000 1.000 0.000 0.000
#&gt; GSM49614     3  0.0000     1.0000 0.000 0.000 1.000 0.000
#&gt; GSM49615     3  0.0000     1.0000 0.000 0.000 1.000 0.000
#&gt; GSM49616     3  0.0000     1.0000 0.000 0.000 1.000 0.000
#&gt; GSM49617     3  0.0000     1.0000 0.000 0.000 1.000 0.000
#&gt; GSM49564     4  0.4713     0.5815 0.360 0.000 0.000 0.640
#&gt; GSM49565     1  0.0469     0.7329 0.988 0.000 0.000 0.012
#&gt; GSM49566     1  0.4746     0.4501 0.632 0.000 0.000 0.368
#&gt; GSM49567     1  0.0469     0.7285 0.988 0.000 0.000 0.012
#&gt; GSM49568     1  0.1022     0.7343 0.968 0.000 0.000 0.032
#&gt; GSM49569     1  0.4790     0.4200 0.620 0.000 0.000 0.380
#&gt; GSM49570     2  0.7896     0.0829 0.000 0.360 0.292 0.348
#&gt; GSM49571     1  0.0817     0.7339 0.976 0.000 0.000 0.024
#&gt; GSM49572     1  0.0469     0.7285 0.988 0.000 0.000 0.012
#&gt; GSM49573     4  0.9352    -0.4803 0.112 0.340 0.192 0.356
#&gt; GSM49574     1  0.0336     0.7310 0.992 0.000 0.000 0.008
#&gt; GSM49575     1  0.0469     0.7285 0.988 0.000 0.000 0.012
#&gt; GSM49576     4  0.4713     0.5815 0.360 0.000 0.000 0.640
#&gt; GSM49577     1  0.1716     0.7205 0.936 0.000 0.000 0.064
#&gt; GSM49578     1  0.0336     0.7320 0.992 0.000 0.000 0.008
#&gt; GSM49579     1  0.4746     0.4501 0.632 0.000 0.000 0.368
#&gt; GSM49580     1  0.0921     0.7350 0.972 0.000 0.000 0.028
#&gt; GSM49581     1  0.0336     0.7320 0.992 0.000 0.000 0.008
#&gt; GSM49582     1  0.0336     0.7320 0.992 0.000 0.000 0.008
#&gt; GSM49583     2  0.0000     0.8999 0.000 1.000 0.000 0.000
#&gt; GSM49584     1  0.0817     0.7351 0.976 0.000 0.000 0.024
#&gt; GSM49585     1  0.2011     0.7160 0.920 0.000 0.000 0.080
#&gt; GSM49586     1  0.4564     0.5026 0.672 0.000 0.000 0.328
#&gt; GSM49587     1  0.0707     0.7352 0.980 0.000 0.000 0.020
#&gt; GSM49588     1  0.3610     0.6337 0.800 0.000 0.000 0.200
#&gt; GSM49589     4  0.4713     0.5815 0.360 0.000 0.000 0.640
#&gt; GSM49590     4  0.5237     0.5751 0.356 0.000 0.016 0.628
#&gt; GSM49591     1  0.0469     0.7347 0.988 0.000 0.000 0.012
#&gt; GSM49592     1  0.0336     0.7320 0.992 0.000 0.000 0.008
#&gt; GSM49593     1  0.4761     0.4500 0.628 0.000 0.000 0.372
#&gt; GSM49594     1  0.4730     0.4576 0.636 0.000 0.000 0.364
#&gt; GSM49595     1  0.4761     0.4506 0.628 0.000 0.000 0.372
#&gt; GSM49596     1  0.4585     0.5001 0.668 0.000 0.000 0.332
#&gt; GSM49597     2  0.7312     0.4059 0.000 0.520 0.188 0.292
#&gt; GSM49598     1  0.4746     0.4501 0.632 0.000 0.000 0.368
#&gt; GSM49599     1  0.0469     0.7285 0.988 0.000 0.000 0.012
#&gt; GSM49600     1  0.2704     0.6920 0.876 0.000 0.000 0.124
#&gt; GSM49601     1  0.4761     0.4506 0.628 0.000 0.000 0.372
#&gt; GSM49602     1  0.4746     0.4501 0.632 0.000 0.000 0.368
#&gt; GSM49603     1  0.4746     0.4501 0.632 0.000 0.000 0.368
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-3-a').click(function(){
  $('#tab-MAD-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-4'>
<p><a id='tab-MAD-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM49613     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM49604     2  0.1341     0.9454 0.000 0.944 0.000 0.056 0.000
#&gt; GSM49605     2  0.0000     0.9885 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49606     2  0.0000     0.9885 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49607     2  0.0000     0.9885 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49608     2  0.0000     0.9885 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49609     2  0.0000     0.9885 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49610     2  0.0000     0.9885 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49611     2  0.0000     0.9885 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49612     2  0.0000     0.9885 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49614     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM49615     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM49616     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM49617     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM49564     5  0.0162     0.3913 0.004 0.000 0.000 0.000 0.996
#&gt; GSM49565     1  0.0404     0.8417 0.988 0.000 0.000 0.012 0.000
#&gt; GSM49566     5  0.4242     0.6450 0.428 0.000 0.000 0.000 0.572
#&gt; GSM49567     1  0.0162     0.8428 0.996 0.000 0.000 0.000 0.004
#&gt; GSM49568     1  0.1043     0.8499 0.960 0.000 0.000 0.000 0.040
#&gt; GSM49569     5  0.4138     0.6456 0.384 0.000 0.000 0.000 0.616
#&gt; GSM49570     4  0.0609     0.9398 0.000 0.000 0.020 0.980 0.000
#&gt; GSM49571     1  0.1012     0.8357 0.968 0.000 0.000 0.012 0.020
#&gt; GSM49572     1  0.0162     0.8435 0.996 0.000 0.000 0.004 0.000
#&gt; GSM49573     4  0.0671     0.9382 0.016 0.000 0.004 0.980 0.000
#&gt; GSM49574     1  0.0290     0.8428 0.992 0.000 0.000 0.008 0.000
#&gt; GSM49575     1  0.0162     0.8428 0.996 0.000 0.000 0.000 0.004
#&gt; GSM49576     5  0.0162     0.3913 0.004 0.000 0.000 0.000 0.996
#&gt; GSM49577     1  0.2006     0.7768 0.916 0.000 0.000 0.012 0.072
#&gt; GSM49578     1  0.1410     0.8435 0.940 0.000 0.000 0.000 0.060
#&gt; GSM49579     5  0.4242     0.6450 0.428 0.000 0.000 0.000 0.572
#&gt; GSM49580     1  0.1282     0.8489 0.952 0.000 0.000 0.004 0.044
#&gt; GSM49581     1  0.1638     0.8419 0.932 0.000 0.000 0.004 0.064
#&gt; GSM49582     1  0.1571     0.8425 0.936 0.000 0.000 0.004 0.060
#&gt; GSM49583     2  0.1121     0.9561 0.000 0.956 0.000 0.044 0.000
#&gt; GSM49584     1  0.1571     0.8425 0.936 0.000 0.000 0.004 0.060
#&gt; GSM49585     1  0.2136     0.7787 0.904 0.000 0.000 0.008 0.088
#&gt; GSM49586     1  0.4597    -0.4588 0.564 0.000 0.000 0.012 0.424
#&gt; GSM49587     1  0.0703     0.8492 0.976 0.000 0.000 0.000 0.024
#&gt; GSM49588     1  0.4084     0.0304 0.668 0.000 0.000 0.004 0.328
#&gt; GSM49589     5  0.0162     0.3913 0.004 0.000 0.000 0.000 0.996
#&gt; GSM49590     5  0.0162     0.3913 0.004 0.000 0.000 0.000 0.996
#&gt; GSM49591     1  0.1697     0.8443 0.932 0.000 0.000 0.008 0.060
#&gt; GSM49592     1  0.1697     0.8443 0.932 0.000 0.000 0.008 0.060
#&gt; GSM49593     5  0.4300     0.6244 0.476 0.000 0.000 0.000 0.524
#&gt; GSM49594     5  0.4562     0.6030 0.492 0.000 0.000 0.008 0.500
#&gt; GSM49595     5  0.4451     0.6069 0.492 0.000 0.000 0.004 0.504
#&gt; GSM49596     1  0.4449    -0.5835 0.512 0.000 0.000 0.004 0.484
#&gt; GSM49597     4  0.2110     0.9036 0.000 0.072 0.016 0.912 0.000
#&gt; GSM49598     5  0.4300     0.5528 0.476 0.000 0.000 0.000 0.524
#&gt; GSM49599     1  0.0162     0.8428 0.996 0.000 0.000 0.000 0.004
#&gt; GSM49600     1  0.1704     0.8392 0.928 0.000 0.000 0.004 0.068
#&gt; GSM49601     5  0.4446     0.6228 0.476 0.000 0.000 0.004 0.520
#&gt; GSM49602     5  0.4242     0.6450 0.428 0.000 0.000 0.000 0.572
#&gt; GSM49603     5  0.4268     0.6401 0.444 0.000 0.000 0.000 0.556
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-4-a').click(function(){
  $('#tab-MAD-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-5'>
<p><a id='tab-MAD-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5 p6
#&gt; GSM49613     3  0.0000     0.9990 0.000 0.000 1.000 0.000 0.000 NA
#&gt; GSM49604     2  0.1391     0.8258 0.000 0.944 0.000 0.040 0.000 NA
#&gt; GSM49605     2  0.2378     0.8557 0.000 0.848 0.000 0.000 0.000 NA
#&gt; GSM49606     2  0.3244     0.8379 0.000 0.732 0.000 0.000 0.000 NA
#&gt; GSM49607     2  0.0000     0.8561 0.000 1.000 0.000 0.000 0.000 NA
#&gt; GSM49608     2  0.0000     0.8561 0.000 1.000 0.000 0.000 0.000 NA
#&gt; GSM49609     2  0.3244     0.8379 0.000 0.732 0.000 0.000 0.000 NA
#&gt; GSM49610     2  0.0000     0.8561 0.000 1.000 0.000 0.000 0.000 NA
#&gt; GSM49611     2  0.3244     0.8379 0.000 0.732 0.000 0.000 0.000 NA
#&gt; GSM49612     2  0.3244     0.8379 0.000 0.732 0.000 0.000 0.000 NA
#&gt; GSM49614     3  0.0146     0.9959 0.000 0.000 0.996 0.004 0.000 NA
#&gt; GSM49615     3  0.0000     0.9990 0.000 0.000 1.000 0.000 0.000 NA
#&gt; GSM49616     3  0.0000     0.9990 0.000 0.000 1.000 0.000 0.000 NA
#&gt; GSM49617     3  0.0000     0.9990 0.000 0.000 1.000 0.000 0.000 NA
#&gt; GSM49564     5  0.2902     0.4585 0.004 0.000 0.000 0.000 0.800 NA
#&gt; GSM49565     1  0.4310     0.6106 0.580 0.000 0.000 0.000 0.024 NA
#&gt; GSM49566     5  0.2912     0.6816 0.216 0.000 0.000 0.000 0.784 NA
#&gt; GSM49567     1  0.3101     0.6738 0.756 0.000 0.000 0.000 0.000 NA
#&gt; GSM49568     1  0.3283     0.5376 0.804 0.000 0.000 0.000 0.160 NA
#&gt; GSM49569     5  0.2793     0.6781 0.200 0.000 0.000 0.000 0.800 NA
#&gt; GSM49570     4  0.0000     0.9198 0.000 0.000 0.000 1.000 0.000 NA
#&gt; GSM49571     1  0.4010     0.6203 0.584 0.000 0.000 0.000 0.008 NA
#&gt; GSM49572     1  0.3555     0.6693 0.712 0.000 0.000 0.000 0.008 NA
#&gt; GSM49573     4  0.0000     0.9198 0.000 0.000 0.000 1.000 0.000 NA
#&gt; GSM49574     1  0.3899     0.6404 0.628 0.000 0.000 0.000 0.008 NA
#&gt; GSM49575     1  0.3101     0.6736 0.756 0.000 0.000 0.000 0.000 NA
#&gt; GSM49576     5  0.2730     0.4595 0.000 0.000 0.000 0.000 0.808 NA
#&gt; GSM49577     1  0.4726     0.5620 0.528 0.000 0.000 0.000 0.048 NA
#&gt; GSM49578     1  0.0692     0.6678 0.976 0.000 0.000 0.000 0.020 NA
#&gt; GSM49579     5  0.3198     0.6796 0.260 0.000 0.000 0.000 0.740 NA
#&gt; GSM49580     1  0.4314     0.4555 0.720 0.000 0.000 0.000 0.184 NA
#&gt; GSM49581     1  0.1970     0.6490 0.900 0.000 0.000 0.000 0.008 NA
#&gt; GSM49582     1  0.1970     0.6490 0.900 0.000 0.000 0.000 0.008 NA
#&gt; GSM49583     2  0.0622     0.8467 0.000 0.980 0.000 0.008 0.000 NA
#&gt; GSM49584     1  0.4403     0.4413 0.708 0.000 0.000 0.000 0.196 NA
#&gt; GSM49585     1  0.5042     0.1748 0.592 0.000 0.000 0.000 0.308 NA
#&gt; GSM49586     5  0.6130    -0.0697 0.324 0.000 0.000 0.000 0.340 NA
#&gt; GSM49587     1  0.1584     0.6736 0.928 0.000 0.000 0.000 0.008 NA
#&gt; GSM49588     5  0.4212     0.4716 0.424 0.000 0.000 0.000 0.560 NA
#&gt; GSM49589     5  0.2762     0.4568 0.000 0.000 0.000 0.000 0.804 NA
#&gt; GSM49590     5  0.2762     0.4568 0.000 0.000 0.000 0.000 0.804 NA
#&gt; GSM49591     1  0.3403     0.6697 0.768 0.000 0.000 0.000 0.020 NA
#&gt; GSM49592     1  0.2199     0.6685 0.892 0.000 0.000 0.000 0.020 NA
#&gt; GSM49593     5  0.3468     0.6735 0.284 0.000 0.000 0.000 0.712 NA
#&gt; GSM49594     5  0.5944     0.2361 0.244 0.000 0.000 0.000 0.452 NA
#&gt; GSM49595     5  0.5629     0.3635 0.224 0.000 0.000 0.000 0.540 NA
#&gt; GSM49596     5  0.3748     0.6613 0.300 0.000 0.000 0.000 0.688 NA
#&gt; GSM49597     4  0.2631     0.8379 0.000 0.152 0.000 0.840 0.000 NA
#&gt; GSM49598     5  0.3830     0.5549 0.376 0.000 0.000 0.000 0.620 NA
#&gt; GSM49599     1  0.3126     0.6729 0.752 0.000 0.000 0.000 0.000 NA
#&gt; GSM49600     1  0.4486     0.4211 0.696 0.000 0.000 0.000 0.208 NA
#&gt; GSM49601     5  0.3936     0.6673 0.288 0.000 0.000 0.000 0.688 NA
#&gt; GSM49602     5  0.3244     0.6775 0.268 0.000 0.000 0.000 0.732 NA
#&gt; GSM49603     5  0.3309     0.6740 0.280 0.000 0.000 0.000 0.720 NA
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-5-a').click(function(){
  $('#tab-MAD-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-get-signatures'>
<ul>
<li><a href='#tab-MAD-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-1-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-1"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-2-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-2"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-3-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-3"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-4-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-4"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-5-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-mclust-signature_compare](figure_cola/MAD-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-mclust-collect-classes](figure_cola/MAD-mclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n disease.state(p) cell.type(p) k
#> MAD:mclust 54         4.59e-06     2.04e-05 2
#> MAD:mclust 54         2.57e-07     8.46e-14 3
#> MAD:mclust 41         5.82e-07     6.34e-10 4
#> MAD:mclust 47         1.93e-07     1.29e-10 5
#> MAD:mclust 42         1.42e-06     3.16e-09 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:NMF






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "NMF"]
# you can also extract it by
# res = res_list["MAD:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21168 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-NMF-collect-plots](figure_cola/MAD-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-NMF-select-partition-number](figure_cola/MAD-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.885           0.920       0.967         0.4341 0.575   0.575
#> 3 3 0.854           0.895       0.949         0.3793 0.758   0.606
#> 4 4 0.620           0.732       0.846         0.1499 0.941   0.860
#> 5 5 0.705           0.693       0.826         0.1055 0.814   0.525
#> 6 6 0.849           0.747       0.874         0.0553 0.924   0.708
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-get-classes'>
<ul>
<li><a href='#tab-MAD-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-NMF-get-classes-1'>
<p><a id='tab-MAD-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM49613     1  0.0000      0.963 1.000 0.000
#&gt; GSM49604     2  0.0000      0.963 0.000 1.000
#&gt; GSM49605     2  0.0000      0.963 0.000 1.000
#&gt; GSM49606     2  0.0000      0.963 0.000 1.000
#&gt; GSM49607     2  0.0000      0.963 0.000 1.000
#&gt; GSM49608     2  0.0000      0.963 0.000 1.000
#&gt; GSM49609     2  0.0000      0.963 0.000 1.000
#&gt; GSM49610     2  0.0000      0.963 0.000 1.000
#&gt; GSM49611     2  0.0000      0.963 0.000 1.000
#&gt; GSM49612     2  0.0000      0.963 0.000 1.000
#&gt; GSM49614     1  0.8386      0.623 0.732 0.268
#&gt; GSM49615     1  0.0000      0.963 1.000 0.000
#&gt; GSM49616     1  0.0000      0.963 1.000 0.000
#&gt; GSM49617     1  0.0000      0.963 1.000 0.000
#&gt; GSM49564     1  0.0000      0.963 1.000 0.000
#&gt; GSM49565     1  0.9954      0.142 0.540 0.460
#&gt; GSM49566     1  0.0000      0.963 1.000 0.000
#&gt; GSM49567     1  0.0938      0.955 0.988 0.012
#&gt; GSM49568     1  0.0000      0.963 1.000 0.000
#&gt; GSM49569     1  0.0000      0.963 1.000 0.000
#&gt; GSM49570     2  0.0000      0.963 0.000 1.000
#&gt; GSM49571     2  0.3431      0.911 0.064 0.936
#&gt; GSM49572     1  0.0672      0.958 0.992 0.008
#&gt; GSM49573     2  0.0000      0.963 0.000 1.000
#&gt; GSM49574     1  0.7056      0.760 0.808 0.192
#&gt; GSM49575     1  0.2043      0.939 0.968 0.032
#&gt; GSM49576     1  0.0000      0.963 1.000 0.000
#&gt; GSM49577     2  0.5946      0.823 0.144 0.856
#&gt; GSM49578     1  0.0000      0.963 1.000 0.000
#&gt; GSM49579     1  0.0000      0.963 1.000 0.000
#&gt; GSM49580     1  0.0000      0.963 1.000 0.000
#&gt; GSM49581     1  0.0000      0.963 1.000 0.000
#&gt; GSM49582     1  0.0000      0.963 1.000 0.000
#&gt; GSM49583     2  0.0000      0.963 0.000 1.000
#&gt; GSM49584     1  0.0000      0.963 1.000 0.000
#&gt; GSM49585     1  0.0000      0.963 1.000 0.000
#&gt; GSM49586     1  0.7602      0.718 0.780 0.220
#&gt; GSM49587     1  0.0000      0.963 1.000 0.000
#&gt; GSM49588     1  0.0000      0.963 1.000 0.000
#&gt; GSM49589     1  0.0000      0.963 1.000 0.000
#&gt; GSM49590     1  0.0000      0.963 1.000 0.000
#&gt; GSM49591     1  0.0000      0.963 1.000 0.000
#&gt; GSM49592     1  0.0000      0.963 1.000 0.000
#&gt; GSM49593     1  0.0000      0.963 1.000 0.000
#&gt; GSM49594     2  0.8909      0.545 0.308 0.692
#&gt; GSM49595     1  0.4431      0.881 0.908 0.092
#&gt; GSM49596     1  0.0000      0.963 1.000 0.000
#&gt; GSM49597     2  0.0000      0.963 0.000 1.000
#&gt; GSM49598     1  0.0000      0.963 1.000 0.000
#&gt; GSM49599     1  0.0672      0.958 0.992 0.008
#&gt; GSM49600     1  0.0000      0.963 1.000 0.000
#&gt; GSM49601     1  0.0000      0.963 1.000 0.000
#&gt; GSM49602     1  0.0000      0.963 1.000 0.000
#&gt; GSM49603     1  0.0000      0.963 1.000 0.000
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-1-a').click(function(){
  $('#tab-MAD-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-2'>
<p><a id='tab-MAD-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM49613     3  0.0000      0.943 0.000 0.000 1.000
#&gt; GSM49604     2  0.0000      0.953 0.000 1.000 0.000
#&gt; GSM49605     2  0.0000      0.953 0.000 1.000 0.000
#&gt; GSM49606     2  0.0000      0.953 0.000 1.000 0.000
#&gt; GSM49607     2  0.0000      0.953 0.000 1.000 0.000
#&gt; GSM49608     2  0.0424      0.947 0.000 0.992 0.008
#&gt; GSM49609     2  0.0000      0.953 0.000 1.000 0.000
#&gt; GSM49610     2  0.0000      0.953 0.000 1.000 0.000
#&gt; GSM49611     2  0.0000      0.953 0.000 1.000 0.000
#&gt; GSM49612     2  0.0000      0.953 0.000 1.000 0.000
#&gt; GSM49614     3  0.0892      0.934 0.000 0.020 0.980
#&gt; GSM49615     3  0.0000      0.943 0.000 0.000 1.000
#&gt; GSM49616     3  0.0424      0.940 0.000 0.008 0.992
#&gt; GSM49617     3  0.0892      0.934 0.000 0.020 0.980
#&gt; GSM49564     3  0.1289      0.937 0.032 0.000 0.968
#&gt; GSM49565     1  0.1753      0.913 0.952 0.048 0.000
#&gt; GSM49566     1  0.3192      0.878 0.888 0.000 0.112
#&gt; GSM49567     1  0.0747      0.929 0.984 0.016 0.000
#&gt; GSM49568     1  0.0592      0.934 0.988 0.000 0.012
#&gt; GSM49569     1  0.3267      0.874 0.884 0.000 0.116
#&gt; GSM49570     2  0.5254      0.629 0.000 0.736 0.264
#&gt; GSM49571     2  0.4974      0.645 0.236 0.764 0.000
#&gt; GSM49572     1  0.0892      0.928 0.980 0.020 0.000
#&gt; GSM49573     2  0.0237      0.950 0.004 0.996 0.000
#&gt; GSM49574     1  0.1163      0.925 0.972 0.028 0.000
#&gt; GSM49575     1  0.1163      0.925 0.972 0.028 0.000
#&gt; GSM49576     3  0.4504      0.750 0.196 0.000 0.804
#&gt; GSM49577     1  0.5926      0.458 0.644 0.356 0.000
#&gt; GSM49578     1  0.0237      0.934 0.996 0.000 0.004
#&gt; GSM49579     1  0.0747      0.934 0.984 0.000 0.016
#&gt; GSM49580     1  0.1289      0.929 0.968 0.000 0.032
#&gt; GSM49581     1  0.0592      0.934 0.988 0.000 0.012
#&gt; GSM49582     1  0.0592      0.934 0.988 0.000 0.012
#&gt; GSM49583     2  0.0000      0.953 0.000 1.000 0.000
#&gt; GSM49584     1  0.2448      0.906 0.924 0.000 0.076
#&gt; GSM49585     1  0.0237      0.934 0.996 0.000 0.004
#&gt; GSM49586     1  0.2537      0.887 0.920 0.080 0.000
#&gt; GSM49587     1  0.0237      0.934 0.996 0.000 0.004
#&gt; GSM49588     1  0.0747      0.934 0.984 0.000 0.016
#&gt; GSM49589     3  0.1964      0.918 0.056 0.000 0.944
#&gt; GSM49590     3  0.1031      0.940 0.024 0.000 0.976
#&gt; GSM49591     1  0.0000      0.933 1.000 0.000 0.000
#&gt; GSM49592     1  0.0000      0.933 1.000 0.000 0.000
#&gt; GSM49593     1  0.2066      0.916 0.940 0.000 0.060
#&gt; GSM49594     1  0.6225      0.255 0.568 0.432 0.000
#&gt; GSM49595     1  0.1411      0.920 0.964 0.036 0.000
#&gt; GSM49596     1  0.0892      0.933 0.980 0.000 0.020
#&gt; GSM49597     2  0.0237      0.950 0.000 0.996 0.004
#&gt; GSM49598     1  0.2625      0.902 0.916 0.000 0.084
#&gt; GSM49599     1  0.1163      0.925 0.972 0.028 0.000
#&gt; GSM49600     1  0.2711      0.897 0.912 0.000 0.088
#&gt; GSM49601     1  0.0892      0.933 0.980 0.000 0.020
#&gt; GSM49602     1  0.1031      0.932 0.976 0.000 0.024
#&gt; GSM49603     1  0.3267      0.876 0.884 0.000 0.116
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-2-a').click(function(){
  $('#tab-MAD-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-3'>
<p><a id='tab-MAD-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM49613     3  0.0672      0.891 0.008 0.000 0.984 0.008
#&gt; GSM49604     4  0.4522      0.681 0.000 0.320 0.000 0.680
#&gt; GSM49605     2  0.1118      0.846 0.000 0.964 0.000 0.036
#&gt; GSM49606     2  0.0188      0.847 0.000 0.996 0.000 0.004
#&gt; GSM49607     2  0.2466      0.816 0.000 0.900 0.004 0.096
#&gt; GSM49608     2  0.2334      0.823 0.000 0.908 0.004 0.088
#&gt; GSM49609     2  0.0657      0.841 0.000 0.984 0.004 0.012
#&gt; GSM49610     2  0.2011      0.832 0.000 0.920 0.000 0.080
#&gt; GSM49611     2  0.0657      0.841 0.000 0.984 0.004 0.012
#&gt; GSM49612     2  0.0000      0.847 0.000 1.000 0.000 0.000
#&gt; GSM49614     3  0.3545      0.759 0.000 0.008 0.828 0.164
#&gt; GSM49615     3  0.0657      0.890 0.004 0.000 0.984 0.012
#&gt; GSM49616     3  0.1576      0.875 0.004 0.000 0.948 0.048
#&gt; GSM49617     3  0.2611      0.832 0.000 0.008 0.896 0.096
#&gt; GSM49564     3  0.0672      0.889 0.008 0.000 0.984 0.008
#&gt; GSM49565     1  0.2125      0.784 0.920 0.076 0.004 0.000
#&gt; GSM49566     1  0.5653      0.699 0.712 0.000 0.192 0.096
#&gt; GSM49567     1  0.2530      0.748 0.888 0.000 0.000 0.112
#&gt; GSM49568     1  0.0000      0.809 1.000 0.000 0.000 0.000
#&gt; GSM49569     1  0.6198      0.649 0.660 0.000 0.224 0.116
#&gt; GSM49570     4  0.4462      0.763 0.000 0.132 0.064 0.804
#&gt; GSM49571     4  0.5920      0.633 0.168 0.120 0.004 0.708
#&gt; GSM49572     1  0.0524      0.808 0.988 0.004 0.000 0.008
#&gt; GSM49573     4  0.3821      0.776 0.040 0.120 0.000 0.840
#&gt; GSM49574     1  0.0336      0.808 0.992 0.000 0.000 0.008
#&gt; GSM49575     1  0.4898      0.212 0.584 0.000 0.000 0.416
#&gt; GSM49576     3  0.4231      0.766 0.080 0.000 0.824 0.096
#&gt; GSM49577     1  0.4644      0.712 0.788 0.164 0.004 0.044
#&gt; GSM49578     1  0.0188      0.809 0.996 0.000 0.000 0.004
#&gt; GSM49579     1  0.4773      0.750 0.788 0.000 0.092 0.120
#&gt; GSM49580     1  0.1557      0.790 0.944 0.000 0.000 0.056
#&gt; GSM49581     1  0.1716      0.786 0.936 0.000 0.000 0.064
#&gt; GSM49582     1  0.0707      0.805 0.980 0.000 0.000 0.020
#&gt; GSM49583     2  0.4072      0.584 0.000 0.748 0.000 0.252
#&gt; GSM49584     1  0.0524      0.809 0.988 0.000 0.004 0.008
#&gt; GSM49585     1  0.1388      0.810 0.960 0.000 0.012 0.028
#&gt; GSM49586     1  0.6935      0.513 0.552 0.112 0.004 0.332
#&gt; GSM49587     1  0.0188      0.809 0.996 0.000 0.000 0.004
#&gt; GSM49588     1  0.1388      0.808 0.960 0.000 0.028 0.012
#&gt; GSM49589     3  0.2402      0.861 0.012 0.000 0.912 0.076
#&gt; GSM49590     3  0.2737      0.842 0.008 0.000 0.888 0.104
#&gt; GSM49591     1  0.0336      0.810 0.992 0.000 0.000 0.008
#&gt; GSM49592     1  0.0376      0.810 0.992 0.004 0.000 0.004
#&gt; GSM49593     1  0.6204      0.679 0.672 0.000 0.160 0.168
#&gt; GSM49594     2  0.6725      0.318 0.184 0.632 0.004 0.180
#&gt; GSM49595     1  0.7668      0.479 0.532 0.276 0.016 0.176
#&gt; GSM49596     1  0.3301      0.786 0.876 0.000 0.048 0.076
#&gt; GSM49597     4  0.4428      0.711 0.000 0.276 0.004 0.720
#&gt; GSM49598     1  0.5208      0.738 0.748 0.000 0.080 0.172
#&gt; GSM49599     1  0.4998     -0.038 0.512 0.000 0.000 0.488
#&gt; GSM49600     1  0.0927      0.811 0.976 0.000 0.016 0.008
#&gt; GSM49601     1  0.7686      0.625 0.604 0.056 0.148 0.192
#&gt; GSM49602     1  0.6808      0.649 0.632 0.008 0.160 0.200
#&gt; GSM49603     1  0.7318      0.485 0.524 0.000 0.280 0.196
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-3-a').click(function(){
  $('#tab-MAD-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-4'>
<p><a id='tab-MAD-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM49613     3  0.0880    0.89873 0.000 0.000 0.968 0.000 0.032
#&gt; GSM49604     4  0.3578    0.64453 0.000 0.132 0.000 0.820 0.048
#&gt; GSM49605     2  0.0992    0.85103 0.000 0.968 0.000 0.024 0.008
#&gt; GSM49606     2  0.0324    0.85034 0.000 0.992 0.000 0.004 0.004
#&gt; GSM49607     2  0.4657    0.75110 0.000 0.752 0.004 0.116 0.128
#&gt; GSM49608     2  0.4400    0.77207 0.000 0.780 0.008 0.108 0.104
#&gt; GSM49609     2  0.1732    0.82184 0.000 0.920 0.000 0.000 0.080
#&gt; GSM49610     2  0.2141    0.84295 0.000 0.916 0.004 0.064 0.016
#&gt; GSM49611     2  0.1732    0.82184 0.000 0.920 0.000 0.000 0.080
#&gt; GSM49612     2  0.1041    0.84473 0.000 0.964 0.004 0.000 0.032
#&gt; GSM49614     3  0.3682    0.74770 0.000 0.012 0.836 0.088 0.064
#&gt; GSM49615     3  0.0703    0.89886 0.000 0.000 0.976 0.000 0.024
#&gt; GSM49616     3  0.0609    0.89810 0.000 0.000 0.980 0.000 0.020
#&gt; GSM49617     3  0.0566    0.88382 0.000 0.000 0.984 0.012 0.004
#&gt; GSM49564     3  0.1121    0.88843 0.000 0.000 0.956 0.000 0.044
#&gt; GSM49565     1  0.2130    0.82397 0.908 0.080 0.000 0.000 0.012
#&gt; GSM49566     1  0.5647    0.04772 0.548 0.000 0.072 0.004 0.376
#&gt; GSM49567     1  0.1372    0.85836 0.956 0.004 0.000 0.016 0.024
#&gt; GSM49568     1  0.0000    0.87588 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49569     5  0.6206    0.62318 0.252 0.000 0.200 0.000 0.548
#&gt; GSM49570     4  0.0727    0.69208 0.000 0.004 0.004 0.980 0.012
#&gt; GSM49571     4  0.4476    0.67276 0.048 0.016 0.000 0.764 0.172
#&gt; GSM49572     1  0.0740    0.87250 0.980 0.008 0.000 0.004 0.008
#&gt; GSM49573     4  0.2570    0.70045 0.004 0.008 0.000 0.880 0.108
#&gt; GSM49574     1  0.0000    0.87588 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49575     1  0.5113    0.30874 0.620 0.000 0.000 0.324 0.056
#&gt; GSM49576     5  0.5587    0.42604 0.072 0.000 0.428 0.000 0.500
#&gt; GSM49577     1  0.6813    0.16111 0.504 0.240 0.000 0.016 0.240
#&gt; GSM49578     1  0.0000    0.87588 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49579     5  0.4803    0.30427 0.444 0.000 0.020 0.000 0.536
#&gt; GSM49580     1  0.0404    0.87494 0.988 0.000 0.000 0.000 0.012
#&gt; GSM49581     1  0.0898    0.86963 0.972 0.000 0.000 0.020 0.008
#&gt; GSM49582     1  0.0162    0.87544 0.996 0.000 0.000 0.000 0.004
#&gt; GSM49583     2  0.6534    0.53467 0.008 0.568 0.012 0.256 0.156
#&gt; GSM49584     1  0.0579    0.87527 0.984 0.000 0.008 0.000 0.008
#&gt; GSM49585     1  0.2741    0.77416 0.860 0.000 0.004 0.004 0.132
#&gt; GSM49586     5  0.4188    0.45882 0.020 0.008 0.000 0.228 0.744
#&gt; GSM49587     1  0.0000    0.87588 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49588     1  0.0898    0.87156 0.972 0.000 0.008 0.000 0.020
#&gt; GSM49589     3  0.3305    0.60508 0.000 0.000 0.776 0.000 0.224
#&gt; GSM49590     5  0.4283    0.32821 0.000 0.000 0.456 0.000 0.544
#&gt; GSM49591     1  0.0880    0.86685 0.968 0.000 0.000 0.000 0.032
#&gt; GSM49592     1  0.0162    0.87550 0.996 0.000 0.000 0.000 0.004
#&gt; GSM49593     5  0.5628    0.67307 0.220 0.000 0.148 0.000 0.632
#&gt; GSM49594     5  0.4752    0.50159 0.036 0.316 0.000 0.000 0.648
#&gt; GSM49595     5  0.4876    0.62614 0.080 0.220 0.000 0.000 0.700
#&gt; GSM49596     1  0.3586    0.53706 0.736 0.000 0.000 0.000 0.264
#&gt; GSM49597     4  0.5743    0.40116 0.000 0.156 0.016 0.664 0.164
#&gt; GSM49598     5  0.5054    0.62153 0.184 0.000 0.004 0.104 0.708
#&gt; GSM49599     4  0.4655    0.00372 0.476 0.000 0.000 0.512 0.012
#&gt; GSM49600     1  0.0992    0.86886 0.968 0.000 0.008 0.000 0.024
#&gt; GSM49601     5  0.5775    0.66011 0.084 0.068 0.152 0.000 0.696
#&gt; GSM49602     5  0.4884    0.69244 0.128 0.000 0.152 0.000 0.720
#&gt; GSM49603     5  0.4519    0.63707 0.052 0.000 0.228 0.000 0.720
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-4-a').click(function(){
  $('#tab-MAD-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-5'>
<p><a id='tab-MAD-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM49613     3  0.0777     0.9062 0.000 0.000 0.972 0.000 0.024 0.004
#&gt; GSM49604     4  0.3254     0.6664 0.000 0.124 0.000 0.820 0.000 0.056
#&gt; GSM49605     2  0.2053     0.8314 0.000 0.888 0.000 0.000 0.004 0.108
#&gt; GSM49606     2  0.1141     0.8573 0.000 0.948 0.000 0.000 0.000 0.052
#&gt; GSM49607     2  0.3652     0.5455 0.000 0.672 0.004 0.000 0.000 0.324
#&gt; GSM49608     2  0.3368     0.6999 0.000 0.756 0.012 0.000 0.000 0.232
#&gt; GSM49609     2  0.0937     0.8316 0.000 0.960 0.000 0.000 0.000 0.040
#&gt; GSM49610     2  0.0937     0.8589 0.000 0.960 0.000 0.000 0.000 0.040
#&gt; GSM49611     2  0.0790     0.8376 0.000 0.968 0.000 0.000 0.000 0.032
#&gt; GSM49612     2  0.0000     0.8524 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49614     3  0.1701     0.8590 0.000 0.000 0.920 0.008 0.000 0.072
#&gt; GSM49615     3  0.0458     0.9075 0.000 0.000 0.984 0.000 0.016 0.000
#&gt; GSM49616     3  0.0291     0.9046 0.000 0.000 0.992 0.000 0.004 0.004
#&gt; GSM49617     3  0.0713     0.8958 0.000 0.000 0.972 0.000 0.000 0.028
#&gt; GSM49564     3  0.1080     0.9011 0.004 0.000 0.960 0.000 0.032 0.004
#&gt; GSM49565     1  0.1398     0.8618 0.940 0.008 0.000 0.000 0.000 0.052
#&gt; GSM49566     5  0.5212     0.5273 0.124 0.004 0.004 0.000 0.632 0.236
#&gt; GSM49567     1  0.1471     0.8558 0.932 0.000 0.000 0.000 0.004 0.064
#&gt; GSM49568     1  0.0146     0.8718 0.996 0.000 0.000 0.000 0.000 0.004
#&gt; GSM49569     5  0.1483     0.8509 0.036 0.000 0.008 0.000 0.944 0.012
#&gt; GSM49570     4  0.3772     0.5413 0.000 0.004 0.004 0.672 0.000 0.320
#&gt; GSM49571     4  0.1340     0.7126 0.000 0.004 0.000 0.948 0.008 0.040
#&gt; GSM49572     1  0.1075     0.8635 0.952 0.000 0.000 0.000 0.000 0.048
#&gt; GSM49573     4  0.1814     0.7188 0.000 0.000 0.000 0.900 0.000 0.100
#&gt; GSM49574     1  0.0622     0.8713 0.980 0.000 0.000 0.008 0.000 0.012
#&gt; GSM49575     1  0.4161     0.2740 0.540 0.000 0.000 0.448 0.000 0.012
#&gt; GSM49576     5  0.1616     0.8504 0.000 0.000 0.020 0.000 0.932 0.048
#&gt; GSM49577     5  0.6755     0.0195 0.156 0.072 0.000 0.000 0.424 0.348
#&gt; GSM49578     1  0.0000     0.8714 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM49579     5  0.2537     0.8091 0.032 0.000 0.000 0.000 0.872 0.096
#&gt; GSM49580     1  0.0858     0.8691 0.968 0.000 0.000 0.004 0.000 0.028
#&gt; GSM49581     1  0.1167     0.8707 0.960 0.000 0.000 0.020 0.008 0.012
#&gt; GSM49582     1  0.0622     0.8708 0.980 0.000 0.000 0.008 0.000 0.012
#&gt; GSM49583     6  0.3862     0.4999 0.004 0.268 0.000 0.004 0.012 0.712
#&gt; GSM49584     1  0.0653     0.8721 0.980 0.000 0.004 0.004 0.000 0.012
#&gt; GSM49585     1  0.4783     0.6997 0.740 0.008 0.008 0.136 0.016 0.092
#&gt; GSM49586     4  0.5025     0.5080 0.000 0.016 0.004 0.684 0.192 0.104
#&gt; GSM49587     1  0.0260     0.8715 0.992 0.000 0.000 0.000 0.000 0.008
#&gt; GSM49588     1  0.1503     0.8626 0.944 0.000 0.000 0.008 0.016 0.032
#&gt; GSM49589     3  0.3512     0.5992 0.000 0.000 0.720 0.000 0.272 0.008
#&gt; GSM49590     5  0.1088     0.8566 0.000 0.000 0.016 0.000 0.960 0.024
#&gt; GSM49591     1  0.2680     0.8305 0.880 0.000 0.000 0.060 0.012 0.048
#&gt; GSM49592     1  0.2121     0.8476 0.916 0.000 0.008 0.040 0.004 0.032
#&gt; GSM49593     5  0.1401     0.8510 0.020 0.000 0.028 0.004 0.948 0.000
#&gt; GSM49594     5  0.1092     0.8512 0.000 0.020 0.000 0.000 0.960 0.020
#&gt; GSM49595     5  0.0405     0.8563 0.000 0.004 0.000 0.000 0.988 0.008
#&gt; GSM49596     1  0.4097    -0.0233 0.504 0.000 0.000 0.000 0.488 0.008
#&gt; GSM49597     6  0.3880     0.5115 0.000 0.024 0.000 0.132 0.052 0.792
#&gt; GSM49598     5  0.1908     0.8433 0.020 0.000 0.012 0.044 0.924 0.000
#&gt; GSM49599     1  0.4855     0.2806 0.556 0.000 0.000 0.380 0.000 0.064
#&gt; GSM49600     1  0.1390     0.8637 0.948 0.000 0.000 0.004 0.032 0.016
#&gt; GSM49601     5  0.5045     0.6761 0.000 0.056 0.068 0.040 0.748 0.088
#&gt; GSM49602     5  0.0291     0.8570 0.004 0.000 0.000 0.000 0.992 0.004
#&gt; GSM49603     5  0.1440     0.8477 0.004 0.000 0.032 0.012 0.948 0.004
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-5-a').click(function(){
  $('#tab-MAD-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-get-signatures'>
<ul>
<li><a href='#tab-MAD-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-1-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-1"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-2-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-2"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-3-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-3"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-4-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-4"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-5-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-NMF-signature_compare](figure_cola/MAD-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-NMF-collect-classes](figure_cola/MAD-NMF-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>          n disease.state(p) cell.type(p) k
#> MAD:NMF 53         2.77e-05     3.04e-03 2
#> MAD:NMF 52         2.78e-06     1.35e-07 3
#> MAD:NMF 49         3.02e-07     9.05e-08 4
#> MAD:NMF 45         3.77e-06     1.19e-07 5
#> MAD:NMF 49         2.61e-07     2.78e-08 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:hclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "hclust"]
# you can also extract it by
# res = res_list["ATC:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21168 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-hclust-collect-plots](figure_cola/ATC-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-hclust-select-partition-number](figure_cola/ATC-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.978       0.987         0.3655 0.628   0.628
#> 3 3 0.908           0.920       0.951         0.1163 0.971   0.953
#> 4 4 1.000           0.966       0.986         0.0308 0.992   0.986
#> 5 5 0.656           0.835       0.890         0.3216 0.874   0.787
#> 6 6 0.695           0.844       0.886         0.0682 0.971   0.937
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-get-classes'>
<ul>
<li><a href='#tab-ATC-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-hclust-get-classes-1'>
<p><a id='tab-ATC-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM49613     1   0.000      0.994 1.000 0.000
#&gt; GSM49604     2   0.506      0.910 0.112 0.888
#&gt; GSM49605     2   0.000      0.958 0.000 1.000
#&gt; GSM49606     2   0.456      0.927 0.096 0.904
#&gt; GSM49607     2   0.000      0.958 0.000 1.000
#&gt; GSM49608     2   0.000      0.958 0.000 1.000
#&gt; GSM49609     2   0.456      0.927 0.096 0.904
#&gt; GSM49610     2   0.000      0.958 0.000 1.000
#&gt; GSM49611     2   0.456      0.927 0.096 0.904
#&gt; GSM49612     2   0.456      0.927 0.096 0.904
#&gt; GSM49614     1   0.000      0.994 1.000 0.000
#&gt; GSM49615     1   0.000      0.994 1.000 0.000
#&gt; GSM49616     1   0.000      0.994 1.000 0.000
#&gt; GSM49617     1   0.000      0.994 1.000 0.000
#&gt; GSM49564     1   0.000      0.994 1.000 0.000
#&gt; GSM49565     1   0.000      0.994 1.000 0.000
#&gt; GSM49566     1   0.000      0.994 1.000 0.000
#&gt; GSM49567     1   0.000      0.994 1.000 0.000
#&gt; GSM49568     1   0.000      0.994 1.000 0.000
#&gt; GSM49569     1   0.000      0.994 1.000 0.000
#&gt; GSM49570     2   0.000      0.958 0.000 1.000
#&gt; GSM49571     1   0.482      0.880 0.896 0.104
#&gt; GSM49572     1   0.000      0.994 1.000 0.000
#&gt; GSM49573     2   0.000      0.958 0.000 1.000
#&gt; GSM49574     1   0.000      0.994 1.000 0.000
#&gt; GSM49575     1   0.000      0.994 1.000 0.000
#&gt; GSM49576     1   0.000      0.994 1.000 0.000
#&gt; GSM49577     1   0.000      0.994 1.000 0.000
#&gt; GSM49578     1   0.000      0.994 1.000 0.000
#&gt; GSM49579     1   0.000      0.994 1.000 0.000
#&gt; GSM49580     1   0.000      0.994 1.000 0.000
#&gt; GSM49581     1   0.000      0.994 1.000 0.000
#&gt; GSM49582     1   0.000      0.994 1.000 0.000
#&gt; GSM49583     2   0.000      0.958 0.000 1.000
#&gt; GSM49584     1   0.000      0.994 1.000 0.000
#&gt; GSM49585     1   0.000      0.994 1.000 0.000
#&gt; GSM49586     1   0.482      0.880 0.896 0.104
#&gt; GSM49587     1   0.000      0.994 1.000 0.000
#&gt; GSM49588     1   0.000      0.994 1.000 0.000
#&gt; GSM49589     1   0.000      0.994 1.000 0.000
#&gt; GSM49590     1   0.000      0.994 1.000 0.000
#&gt; GSM49591     1   0.000      0.994 1.000 0.000
#&gt; GSM49592     1   0.000      0.994 1.000 0.000
#&gt; GSM49593     1   0.000      0.994 1.000 0.000
#&gt; GSM49594     1   0.000      0.994 1.000 0.000
#&gt; GSM49595     1   0.000      0.994 1.000 0.000
#&gt; GSM49596     1   0.000      0.994 1.000 0.000
#&gt; GSM49597     2   0.000      0.958 0.000 1.000
#&gt; GSM49598     1   0.000      0.994 1.000 0.000
#&gt; GSM49599     1   0.000      0.994 1.000 0.000
#&gt; GSM49600     1   0.000      0.994 1.000 0.000
#&gt; GSM49601     1   0.000      0.994 1.000 0.000
#&gt; GSM49602     1   0.000      0.994 1.000 0.000
#&gt; GSM49603     1   0.000      0.994 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-1-a').click(function(){
  $('#tab-ATC-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-2'>
<p><a id='tab-ATC-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM49613     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM49604     2  0.0237      0.543 0.000 0.996 0.004
#&gt; GSM49605     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM49606     2  0.6308      0.496 0.000 0.508 0.492
#&gt; GSM49607     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM49608     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM49609     2  0.6302      0.520 0.000 0.520 0.480
#&gt; GSM49610     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM49611     2  0.6302      0.520 0.000 0.520 0.480
#&gt; GSM49612     2  0.6302      0.520 0.000 0.520 0.480
#&gt; GSM49614     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM49615     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM49616     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM49617     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM49564     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM49565     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM49566     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM49567     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM49568     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM49569     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM49570     2  0.3619      0.531 0.000 0.864 0.136
#&gt; GSM49571     1  0.4750      0.734 0.784 0.216 0.000
#&gt; GSM49572     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM49573     2  0.3619      0.531 0.000 0.864 0.136
#&gt; GSM49574     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM49575     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM49576     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM49577     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM49578     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM49579     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM49580     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM49581     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM49582     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM49583     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM49584     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM49585     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM49586     1  0.4750      0.734 0.784 0.216 0.000
#&gt; GSM49587     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM49588     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM49589     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM49590     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM49591     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM49592     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM49593     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM49594     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM49595     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM49596     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM49597     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM49598     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM49599     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM49600     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM49601     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM49602     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM49603     1  0.0000      0.989 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-2-a').click(function(){
  $('#tab-ATC-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-3'>
<p><a id='tab-ATC-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM49613     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49604     4  0.4406      0.539 0.000 0.000 0.300 0.700
#&gt; GSM49605     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM49606     3  0.0469      0.982 0.000 0.012 0.988 0.000
#&gt; GSM49607     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM49608     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM49609     3  0.0000      0.994 0.000 0.000 1.000 0.000
#&gt; GSM49610     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM49611     3  0.0000      0.994 0.000 0.000 1.000 0.000
#&gt; GSM49612     3  0.0000      0.994 0.000 0.000 1.000 0.000
#&gt; GSM49614     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49615     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49616     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49617     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49564     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49565     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49566     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49567     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49568     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49569     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49570     4  0.0592      0.828 0.000 0.000 0.016 0.984
#&gt; GSM49571     1  0.4175      0.731 0.784 0.000 0.200 0.016
#&gt; GSM49572     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49573     4  0.0592      0.828 0.000 0.000 0.016 0.984
#&gt; GSM49574     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49575     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49576     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49577     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49578     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49579     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49580     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49581     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49582     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49583     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM49584     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49585     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49586     1  0.4175      0.731 0.784 0.000 0.200 0.016
#&gt; GSM49587     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49588     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49589     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49590     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49591     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49592     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49593     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49594     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49595     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49596     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49597     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM49598     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49599     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49600     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49601     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49602     1  0.0000      0.989 1.000 0.000 0.000 0.000
#&gt; GSM49603     1  0.0000      0.989 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-3-a').click(function(){
  $('#tab-ATC-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-4'>
<p><a id='tab-ATC-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM49613     3  0.4192      1.000 0.404 0.000 0.596 0.000 0.000
#&gt; GSM49604     4  0.4121      0.709 0.000 0.000 0.112 0.788 0.100
#&gt; GSM49605     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49606     5  0.0404      0.984 0.000 0.012 0.000 0.000 0.988
#&gt; GSM49607     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49608     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49609     5  0.0000      0.995 0.000 0.000 0.000 0.000 1.000
#&gt; GSM49610     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49611     5  0.0000      0.995 0.000 0.000 0.000 0.000 1.000
#&gt; GSM49612     5  0.0000      0.995 0.000 0.000 0.000 0.000 1.000
#&gt; GSM49614     3  0.4192      1.000 0.404 0.000 0.596 0.000 0.000
#&gt; GSM49615     3  0.4192      1.000 0.404 0.000 0.596 0.000 0.000
#&gt; GSM49616     3  0.4192      1.000 0.404 0.000 0.596 0.000 0.000
#&gt; GSM49617     3  0.4192      1.000 0.404 0.000 0.596 0.000 0.000
#&gt; GSM49564     1  0.4192     -0.548 0.596 0.000 0.404 0.000 0.000
#&gt; GSM49565     1  0.2891      0.816 0.824 0.000 0.000 0.176 0.000
#&gt; GSM49566     1  0.2773      0.822 0.836 0.000 0.000 0.164 0.000
#&gt; GSM49567     1  0.2891      0.816 0.824 0.000 0.000 0.176 0.000
#&gt; GSM49568     1  0.0162      0.832 0.996 0.000 0.000 0.004 0.000
#&gt; GSM49569     1  0.2773      0.822 0.836 0.000 0.000 0.164 0.000
#&gt; GSM49570     4  0.4192      0.869 0.000 0.000 0.404 0.596 0.000
#&gt; GSM49571     1  0.4192      0.494 0.596 0.000 0.000 0.404 0.000
#&gt; GSM49572     1  0.2891      0.816 0.824 0.000 0.000 0.176 0.000
#&gt; GSM49573     4  0.4192      0.869 0.000 0.000 0.404 0.596 0.000
#&gt; GSM49574     1  0.2773      0.822 0.836 0.000 0.000 0.164 0.000
#&gt; GSM49575     1  0.3003      0.806 0.812 0.000 0.000 0.188 0.000
#&gt; GSM49576     1  0.2329      0.829 0.876 0.000 0.000 0.124 0.000
#&gt; GSM49577     1  0.2966      0.810 0.816 0.000 0.000 0.184 0.000
#&gt; GSM49578     1  0.0000      0.831 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49579     1  0.2852      0.818 0.828 0.000 0.000 0.172 0.000
#&gt; GSM49580     1  0.0000      0.831 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49581     1  0.0000      0.831 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49582     1  0.0000      0.831 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49583     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49584     1  0.0000      0.831 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49585     1  0.0000      0.831 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49586     1  0.4192      0.494 0.596 0.000 0.000 0.404 0.000
#&gt; GSM49587     1  0.0510      0.834 0.984 0.000 0.000 0.016 0.000
#&gt; GSM49588     1  0.0000      0.831 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49589     1  0.2329      0.829 0.876 0.000 0.000 0.124 0.000
#&gt; GSM49590     1  0.2773      0.822 0.836 0.000 0.000 0.164 0.000
#&gt; GSM49591     1  0.0000      0.831 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49592     1  0.0000      0.831 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49593     1  0.0000      0.831 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49594     1  0.2891      0.816 0.824 0.000 0.000 0.176 0.000
#&gt; GSM49595     1  0.2891      0.816 0.824 0.000 0.000 0.176 0.000
#&gt; GSM49596     1  0.0794      0.834 0.972 0.000 0.000 0.028 0.000
#&gt; GSM49597     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49598     1  0.0000      0.831 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49599     1  0.2891      0.816 0.824 0.000 0.000 0.176 0.000
#&gt; GSM49600     1  0.0000      0.831 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49601     1  0.0000      0.831 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49602     1  0.0290      0.833 0.992 0.000 0.000 0.008 0.000
#&gt; GSM49603     1  0.0290      0.833 0.992 0.000 0.000 0.008 0.000
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-4-a').click(function(){
  $('#tab-ATC-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-5'>
<p><a id='tab-ATC-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM49613     3  0.2697      0.760 0.188 0.000 0.812 0.000 0.000 0.000
#&gt; GSM49604     5  0.5157      0.000 0.000 0.000 0.024 0.284 0.624 0.068
#&gt; GSM49605     2  0.0000      0.948 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49606     6  0.0363      0.981 0.000 0.012 0.000 0.000 0.000 0.988
#&gt; GSM49607     2  0.0000      0.948 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49608     2  0.0000      0.948 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49609     6  0.0000      0.994 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM49610     2  0.0000      0.948 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49611     6  0.0000      0.994 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM49612     6  0.0000      0.994 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM49614     3  0.0632      0.625 0.024 0.000 0.976 0.000 0.000 0.000
#&gt; GSM49615     3  0.2697      0.760 0.188 0.000 0.812 0.000 0.000 0.000
#&gt; GSM49616     3  0.2664      0.760 0.184 0.000 0.816 0.000 0.000 0.000
#&gt; GSM49617     3  0.0632      0.625 0.024 0.000 0.976 0.000 0.000 0.000
#&gt; GSM49564     3  0.3817      0.408 0.432 0.000 0.568 0.000 0.000 0.000
#&gt; GSM49565     1  0.2738      0.866 0.820 0.000 0.004 0.000 0.176 0.000
#&gt; GSM49566     1  0.2595      0.872 0.836 0.000 0.004 0.000 0.160 0.000
#&gt; GSM49567     1  0.2738      0.866 0.820 0.000 0.004 0.000 0.176 0.000
#&gt; GSM49568     1  0.0603      0.879 0.980 0.000 0.016 0.000 0.004 0.000
#&gt; GSM49569     1  0.2595      0.872 0.836 0.000 0.004 0.000 0.160 0.000
#&gt; GSM49570     4  0.0000      1.000 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM49571     1  0.3890      0.615 0.596 0.000 0.004 0.000 0.400 0.000
#&gt; GSM49572     1  0.2738      0.866 0.820 0.000 0.004 0.000 0.176 0.000
#&gt; GSM49573     4  0.0000      1.000 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM49574     1  0.2595      0.872 0.836 0.000 0.004 0.000 0.160 0.000
#&gt; GSM49575     1  0.2730      0.858 0.808 0.000 0.000 0.000 0.192 0.000
#&gt; GSM49576     1  0.2191      0.879 0.876 0.000 0.004 0.000 0.120 0.000
#&gt; GSM49577     1  0.2838      0.858 0.808 0.000 0.004 0.000 0.188 0.000
#&gt; GSM49578     1  0.0363      0.879 0.988 0.000 0.012 0.000 0.000 0.000
#&gt; GSM49579     1  0.2703      0.867 0.824 0.000 0.004 0.000 0.172 0.000
#&gt; GSM49580     1  0.0363      0.879 0.988 0.000 0.012 0.000 0.000 0.000
#&gt; GSM49581     1  0.0146      0.883 0.996 0.000 0.004 0.000 0.000 0.000
#&gt; GSM49582     1  0.0458      0.876 0.984 0.000 0.016 0.000 0.000 0.000
#&gt; GSM49583     2  0.1910      0.901 0.000 0.892 0.000 0.000 0.108 0.000
#&gt; GSM49584     1  0.0458      0.876 0.984 0.000 0.016 0.000 0.000 0.000
#&gt; GSM49585     1  0.0146      0.883 0.996 0.000 0.004 0.000 0.000 0.000
#&gt; GSM49586     1  0.3890      0.615 0.596 0.000 0.004 0.000 0.400 0.000
#&gt; GSM49587     1  0.0363      0.885 0.988 0.000 0.000 0.000 0.012 0.000
#&gt; GSM49588     1  0.0146      0.883 0.996 0.000 0.004 0.000 0.000 0.000
#&gt; GSM49589     1  0.2048      0.880 0.880 0.000 0.000 0.000 0.120 0.000
#&gt; GSM49590     1  0.2595      0.872 0.836 0.000 0.004 0.000 0.160 0.000
#&gt; GSM49591     1  0.0146      0.883 0.996 0.000 0.004 0.000 0.000 0.000
#&gt; GSM49592     1  0.0146      0.883 0.996 0.000 0.004 0.000 0.000 0.000
#&gt; GSM49593     1  0.0146      0.883 0.996 0.000 0.004 0.000 0.000 0.000
#&gt; GSM49594     1  0.2738      0.866 0.820 0.000 0.004 0.000 0.176 0.000
#&gt; GSM49595     1  0.2738      0.866 0.820 0.000 0.004 0.000 0.176 0.000
#&gt; GSM49596     1  0.0632      0.886 0.976 0.000 0.000 0.000 0.024 0.000
#&gt; GSM49597     2  0.2664      0.847 0.000 0.816 0.000 0.000 0.184 0.000
#&gt; GSM49598     1  0.0146      0.883 0.996 0.000 0.004 0.000 0.000 0.000
#&gt; GSM49599     1  0.2738      0.866 0.820 0.000 0.004 0.000 0.176 0.000
#&gt; GSM49600     1  0.0146      0.883 0.996 0.000 0.004 0.000 0.000 0.000
#&gt; GSM49601     1  0.0146      0.883 0.996 0.000 0.004 0.000 0.000 0.000
#&gt; GSM49602     1  0.0405      0.885 0.988 0.000 0.004 0.000 0.008 0.000
#&gt; GSM49603     1  0.0405      0.885 0.988 0.000 0.004 0.000 0.008 0.000
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-5-a').click(function(){
  $('#tab-ATC-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-get-signatures'>
<ul>
<li><a href='#tab-ATC-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-1-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-1"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-2-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-2"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-3-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-3"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-4-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-4"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-5-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-hclust-signature_compare](figure_cola/ATC-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-hclust-collect-classes](figure_cola/ATC-hclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n disease.state(p) cell.type(p) k
#> ATC:hclust 54         5.97e-07     6.72e-04 2
#> ATC:hclust 53         1.26e-06     4.07e-04 3
#> ATC:hclust 54         1.73e-07     1.60e-05 4
#> ATC:hclust 51         1.07e-06     5.14e-14 5
#> ATC:hclust 52         1.72e-07     2.44e-14 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:kmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "kmeans"]
# you can also extract it by
# res = res_list["ATC:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21168 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-kmeans-collect-plots](figure_cola/ATC-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-kmeans-select-partition-number](figure_cola/ATC-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.997       0.994         0.3705 0.628   0.628
#> 3 3 0.633           0.788       0.857         0.4126 0.874   0.800
#> 4 4 0.615           0.747       0.814         0.1741 0.830   0.662
#> 5 5 0.638           0.731       0.814         0.1362 0.932   0.801
#> 6 6 0.705           0.730       0.837         0.0783 0.886   0.644
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-get-classes'>
<ul>
<li><a href='#tab-ATC-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-kmeans-get-classes-1'>
<p><a id='tab-ATC-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM49613     1   0.118      0.986 0.984 0.016
#&gt; GSM49604     2   0.118      1.000 0.016 0.984
#&gt; GSM49605     2   0.118      1.000 0.016 0.984
#&gt; GSM49606     2   0.118      1.000 0.016 0.984
#&gt; GSM49607     2   0.118      1.000 0.016 0.984
#&gt; GSM49608     2   0.118      1.000 0.016 0.984
#&gt; GSM49609     2   0.118      1.000 0.016 0.984
#&gt; GSM49610     2   0.118      1.000 0.016 0.984
#&gt; GSM49611     2   0.118      1.000 0.016 0.984
#&gt; GSM49612     2   0.118      1.000 0.016 0.984
#&gt; GSM49614     1   0.118      0.986 0.984 0.016
#&gt; GSM49615     1   0.118      0.986 0.984 0.016
#&gt; GSM49616     1   0.118      0.986 0.984 0.016
#&gt; GSM49617     1   0.118      0.986 0.984 0.016
#&gt; GSM49564     1   0.118      0.986 0.984 0.016
#&gt; GSM49565     1   0.000      0.998 1.000 0.000
#&gt; GSM49566     1   0.000      0.998 1.000 0.000
#&gt; GSM49567     1   0.000      0.998 1.000 0.000
#&gt; GSM49568     1   0.000      0.998 1.000 0.000
#&gt; GSM49569     1   0.000      0.998 1.000 0.000
#&gt; GSM49570     2   0.118      1.000 0.016 0.984
#&gt; GSM49571     1   0.000      0.998 1.000 0.000
#&gt; GSM49572     1   0.000      0.998 1.000 0.000
#&gt; GSM49573     2   0.118      1.000 0.016 0.984
#&gt; GSM49574     1   0.000      0.998 1.000 0.000
#&gt; GSM49575     1   0.000      0.998 1.000 0.000
#&gt; GSM49576     1   0.000      0.998 1.000 0.000
#&gt; GSM49577     1   0.000      0.998 1.000 0.000
#&gt; GSM49578     1   0.000      0.998 1.000 0.000
#&gt; GSM49579     1   0.000      0.998 1.000 0.000
#&gt; GSM49580     1   0.000      0.998 1.000 0.000
#&gt; GSM49581     1   0.000      0.998 1.000 0.000
#&gt; GSM49582     1   0.000      0.998 1.000 0.000
#&gt; GSM49583     2   0.118      1.000 0.016 0.984
#&gt; GSM49584     1   0.000      0.998 1.000 0.000
#&gt; GSM49585     1   0.000      0.998 1.000 0.000
#&gt; GSM49586     1   0.000      0.998 1.000 0.000
#&gt; GSM49587     1   0.000      0.998 1.000 0.000
#&gt; GSM49588     1   0.000      0.998 1.000 0.000
#&gt; GSM49589     1   0.000      0.998 1.000 0.000
#&gt; GSM49590     1   0.000      0.998 1.000 0.000
#&gt; GSM49591     1   0.000      0.998 1.000 0.000
#&gt; GSM49592     1   0.000      0.998 1.000 0.000
#&gt; GSM49593     1   0.000      0.998 1.000 0.000
#&gt; GSM49594     1   0.000      0.998 1.000 0.000
#&gt; GSM49595     1   0.000      0.998 1.000 0.000
#&gt; GSM49596     1   0.000      0.998 1.000 0.000
#&gt; GSM49597     2   0.118      1.000 0.016 0.984
#&gt; GSM49598     1   0.000      0.998 1.000 0.000
#&gt; GSM49599     1   0.000      0.998 1.000 0.000
#&gt; GSM49600     1   0.000      0.998 1.000 0.000
#&gt; GSM49601     1   0.000      0.998 1.000 0.000
#&gt; GSM49602     1   0.000      0.998 1.000 0.000
#&gt; GSM49603     1   0.000      0.998 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-1-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-2'>
<p><a id='tab-ATC-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM49613     3   0.470      0.951 0.212 0.000 0.788
#&gt; GSM49604     2   0.470      0.885 0.000 0.788 0.212
#&gt; GSM49605     2   0.000      0.953 0.000 1.000 0.000
#&gt; GSM49606     2   0.116      0.949 0.000 0.972 0.028
#&gt; GSM49607     2   0.000      0.953 0.000 1.000 0.000
#&gt; GSM49608     2   0.000      0.953 0.000 1.000 0.000
#&gt; GSM49609     2   0.296      0.939 0.008 0.912 0.080
#&gt; GSM49610     2   0.000      0.953 0.000 1.000 0.000
#&gt; GSM49611     2   0.254      0.942 0.000 0.920 0.080
#&gt; GSM49612     2   0.254      0.942 0.000 0.920 0.080
#&gt; GSM49614     3   0.559      0.824 0.304 0.000 0.696
#&gt; GSM49615     3   0.470      0.951 0.212 0.000 0.788
#&gt; GSM49616     3   0.470      0.951 0.212 0.000 0.788
#&gt; GSM49617     3   0.480      0.947 0.220 0.000 0.780
#&gt; GSM49564     1   0.565      0.759 0.688 0.000 0.312
#&gt; GSM49565     1   0.000      0.648 1.000 0.000 0.000
#&gt; GSM49566     1   0.480      0.755 0.780 0.000 0.220
#&gt; GSM49567     1   0.000      0.648 1.000 0.000 0.000
#&gt; GSM49568     1   0.518      0.765 0.744 0.000 0.256
#&gt; GSM49569     1   0.465      0.750 0.792 0.000 0.208
#&gt; GSM49570     2   0.382      0.900 0.000 0.852 0.148
#&gt; GSM49571     1   0.429      0.333 0.820 0.000 0.180
#&gt; GSM49572     1   0.000      0.648 1.000 0.000 0.000
#&gt; GSM49573     2   0.429      0.892 0.000 0.820 0.180
#&gt; GSM49574     1   0.000      0.648 1.000 0.000 0.000
#&gt; GSM49575     1   0.000      0.648 1.000 0.000 0.000
#&gt; GSM49576     1   0.550      0.767 0.708 0.000 0.292
#&gt; GSM49577     1   0.000      0.648 1.000 0.000 0.000
#&gt; GSM49578     1   0.565      0.759 0.688 0.000 0.312
#&gt; GSM49579     1   0.460      0.751 0.796 0.000 0.204
#&gt; GSM49580     1   0.559      0.764 0.696 0.000 0.304
#&gt; GSM49581     1   0.245      0.679 0.924 0.000 0.076
#&gt; GSM49582     1   0.565      0.759 0.688 0.000 0.312
#&gt; GSM49583     2   0.000      0.953 0.000 1.000 0.000
#&gt; GSM49584     1   0.565      0.759 0.688 0.000 0.312
#&gt; GSM49585     1   0.559      0.763 0.696 0.000 0.304
#&gt; GSM49586     1   0.000      0.648 1.000 0.000 0.000
#&gt; GSM49587     1   0.565      0.759 0.688 0.000 0.312
#&gt; GSM49588     1   0.565      0.759 0.688 0.000 0.312
#&gt; GSM49589     1   0.565      0.759 0.688 0.000 0.312
#&gt; GSM49590     1   0.480      0.755 0.780 0.000 0.220
#&gt; GSM49591     1   0.493      0.758 0.768 0.000 0.232
#&gt; GSM49592     1   0.565      0.759 0.688 0.000 0.312
#&gt; GSM49593     1   0.559      0.764 0.696 0.000 0.304
#&gt; GSM49594     1   0.000      0.648 1.000 0.000 0.000
#&gt; GSM49595     1   0.000      0.648 1.000 0.000 0.000
#&gt; GSM49596     1   0.550      0.768 0.708 0.000 0.292
#&gt; GSM49597     2   0.000      0.953 0.000 1.000 0.000
#&gt; GSM49598     1   0.497      0.759 0.764 0.000 0.236
#&gt; GSM49599     1   0.000      0.648 1.000 0.000 0.000
#&gt; GSM49600     1   0.559      0.764 0.696 0.000 0.304
#&gt; GSM49601     1   0.565      0.759 0.688 0.000 0.312
#&gt; GSM49602     1   0.550      0.768 0.708 0.000 0.292
#&gt; GSM49603     1   0.556      0.766 0.700 0.000 0.300
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-2-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-3'>
<p><a id='tab-ATC-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM49613     3  0.4605      0.879 0.336 0.000 0.664 0.000
#&gt; GSM49604     2  0.7745      0.541 0.000 0.396 0.232 0.372
#&gt; GSM49605     2  0.0000      0.838 0.000 1.000 0.000 0.000
#&gt; GSM49606     2  0.2329      0.824 0.000 0.916 0.072 0.012
#&gt; GSM49607     2  0.0000      0.838 0.000 1.000 0.000 0.000
#&gt; GSM49608     2  0.0000      0.838 0.000 1.000 0.000 0.000
#&gt; GSM49609     2  0.5140      0.783 0.000 0.760 0.144 0.096
#&gt; GSM49610     2  0.0000      0.838 0.000 1.000 0.000 0.000
#&gt; GSM49611     2  0.4465      0.800 0.000 0.800 0.144 0.056
#&gt; GSM49612     2  0.4614      0.798 0.000 0.792 0.144 0.064
#&gt; GSM49614     3  0.5891      0.790 0.168 0.000 0.700 0.132
#&gt; GSM49615     3  0.4605      0.879 0.336 0.000 0.664 0.000
#&gt; GSM49616     3  0.4250      0.892 0.276 0.000 0.724 0.000
#&gt; GSM49617     3  0.5279      0.876 0.232 0.000 0.716 0.052
#&gt; GSM49564     1  0.0469      0.866 0.988 0.000 0.012 0.000
#&gt; GSM49565     4  0.5000      0.679 0.496 0.000 0.000 0.504
#&gt; GSM49566     1  0.2814      0.720 0.868 0.000 0.000 0.132
#&gt; GSM49567     4  0.4888      0.845 0.412 0.000 0.000 0.588
#&gt; GSM49568     1  0.1474      0.831 0.948 0.000 0.000 0.052
#&gt; GSM49569     1  0.3123      0.670 0.844 0.000 0.000 0.156
#&gt; GSM49570     2  0.7106      0.582 0.000 0.528 0.148 0.324
#&gt; GSM49571     4  0.2759      0.209 0.052 0.000 0.044 0.904
#&gt; GSM49572     1  0.5000     -0.715 0.500 0.000 0.000 0.500
#&gt; GSM49573     2  0.7591      0.554 0.000 0.444 0.204 0.352
#&gt; GSM49574     1  0.5000     -0.704 0.504 0.000 0.000 0.496
#&gt; GSM49575     4  0.4843      0.848 0.396 0.000 0.000 0.604
#&gt; GSM49576     1  0.1302      0.837 0.956 0.000 0.000 0.044
#&gt; GSM49577     4  0.4843      0.848 0.396 0.000 0.000 0.604
#&gt; GSM49578     1  0.0188      0.874 0.996 0.000 0.004 0.000
#&gt; GSM49579     1  0.2973      0.696 0.856 0.000 0.000 0.144
#&gt; GSM49580     1  0.0000      0.874 1.000 0.000 0.000 0.000
#&gt; GSM49581     1  0.2921      0.666 0.860 0.000 0.000 0.140
#&gt; GSM49582     1  0.0188      0.874 0.996 0.000 0.004 0.000
#&gt; GSM49583     2  0.0000      0.838 0.000 1.000 0.000 0.000
#&gt; GSM49584     1  0.0188      0.874 0.996 0.000 0.004 0.000
#&gt; GSM49585     1  0.0188      0.874 0.996 0.000 0.004 0.000
#&gt; GSM49586     4  0.4948      0.797 0.440 0.000 0.000 0.560
#&gt; GSM49587     1  0.0188      0.874 0.996 0.000 0.004 0.000
#&gt; GSM49588     1  0.0188      0.874 0.996 0.000 0.004 0.000
#&gt; GSM49589     1  0.0188      0.874 0.996 0.000 0.004 0.000
#&gt; GSM49590     1  0.2814      0.720 0.868 0.000 0.000 0.132
#&gt; GSM49591     1  0.0921      0.851 0.972 0.000 0.000 0.028
#&gt; GSM49592     1  0.0188      0.874 0.996 0.000 0.004 0.000
#&gt; GSM49593     1  0.0000      0.874 1.000 0.000 0.000 0.000
#&gt; GSM49594     4  0.4843      0.848 0.396 0.000 0.000 0.604
#&gt; GSM49595     4  0.4933      0.822 0.432 0.000 0.000 0.568
#&gt; GSM49596     1  0.0000      0.874 1.000 0.000 0.000 0.000
#&gt; GSM49597     2  0.0000      0.838 0.000 1.000 0.000 0.000
#&gt; GSM49598     1  0.0921      0.851 0.972 0.000 0.000 0.028
#&gt; GSM49599     4  0.4898      0.843 0.416 0.000 0.000 0.584
#&gt; GSM49600     1  0.0000      0.874 1.000 0.000 0.000 0.000
#&gt; GSM49601     1  0.0188      0.874 0.996 0.000 0.004 0.000
#&gt; GSM49602     1  0.0000      0.874 1.000 0.000 0.000 0.000
#&gt; GSM49603     1  0.0000      0.874 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-3-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-4'>
<p><a id='tab-ATC-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM49613     3  0.3355      0.847 0.000 0.000 0.804 0.012 0.184
#&gt; GSM49604     4  0.3564      0.784 0.028 0.100 0.028 0.844 0.000
#&gt; GSM49605     2  0.0000      0.791 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49606     2  0.4088      0.710 0.060 0.824 0.052 0.064 0.000
#&gt; GSM49607     2  0.0162      0.790 0.004 0.996 0.000 0.000 0.000
#&gt; GSM49608     2  0.0000      0.791 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49609     2  0.6645      0.475 0.072 0.560 0.076 0.292 0.000
#&gt; GSM49610     2  0.0000      0.791 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49611     2  0.6593      0.481 0.068 0.564 0.076 0.292 0.000
#&gt; GSM49612     2  0.6593      0.481 0.068 0.564 0.076 0.292 0.000
#&gt; GSM49614     3  0.3905      0.812 0.080 0.000 0.832 0.036 0.052
#&gt; GSM49615     3  0.3355      0.847 0.000 0.000 0.804 0.012 0.184
#&gt; GSM49616     3  0.2074      0.870 0.000 0.000 0.896 0.000 0.104
#&gt; GSM49617     3  0.3073      0.857 0.052 0.000 0.868 0.004 0.076
#&gt; GSM49564     5  0.2430      0.836 0.028 0.000 0.020 0.040 0.912
#&gt; GSM49565     1  0.3990      0.744 0.688 0.000 0.000 0.004 0.308
#&gt; GSM49566     5  0.5298      0.206 0.396 0.000 0.004 0.044 0.556
#&gt; GSM49567     1  0.3048      0.819 0.820 0.000 0.000 0.004 0.176
#&gt; GSM49568     5  0.1251      0.841 0.036 0.000 0.000 0.008 0.956
#&gt; GSM49569     5  0.5383      0.161 0.408 0.000 0.004 0.048 0.540
#&gt; GSM49570     4  0.3774      0.724 0.000 0.296 0.000 0.704 0.000
#&gt; GSM49571     1  0.4937      0.102 0.604 0.000 0.028 0.364 0.004
#&gt; GSM49572     1  0.4290      0.744 0.680 0.000 0.000 0.016 0.304
#&gt; GSM49573     4  0.2773      0.833 0.000 0.164 0.000 0.836 0.000
#&gt; GSM49574     1  0.4066      0.726 0.672 0.000 0.000 0.004 0.324
#&gt; GSM49575     1  0.3398      0.782 0.828 0.000 0.024 0.004 0.144
#&gt; GSM49576     5  0.4713      0.517 0.280 0.000 0.000 0.044 0.676
#&gt; GSM49577     1  0.2773      0.813 0.836 0.000 0.000 0.000 0.164
#&gt; GSM49578     5  0.0404      0.855 0.000 0.000 0.000 0.012 0.988
#&gt; GSM49579     5  0.5118      0.174 0.412 0.000 0.000 0.040 0.548
#&gt; GSM49580     5  0.0290      0.856 0.000 0.000 0.000 0.008 0.992
#&gt; GSM49581     5  0.1444      0.832 0.040 0.000 0.000 0.012 0.948
#&gt; GSM49582     5  0.0404      0.855 0.000 0.000 0.000 0.012 0.988
#&gt; GSM49583     2  0.0290      0.789 0.008 0.992 0.000 0.000 0.000
#&gt; GSM49584     5  0.0510      0.855 0.000 0.000 0.000 0.016 0.984
#&gt; GSM49585     5  0.0566      0.856 0.004 0.000 0.000 0.012 0.984
#&gt; GSM49586     1  0.4970      0.532 0.624 0.000 0.028 0.008 0.340
#&gt; GSM49587     5  0.0290      0.855 0.000 0.000 0.000 0.008 0.992
#&gt; GSM49588     5  0.0566      0.856 0.004 0.000 0.000 0.012 0.984
#&gt; GSM49589     5  0.1915      0.843 0.032 0.000 0.000 0.040 0.928
#&gt; GSM49590     5  0.5261      0.266 0.380 0.000 0.004 0.044 0.572
#&gt; GSM49591     5  0.0807      0.855 0.012 0.000 0.000 0.012 0.976
#&gt; GSM49592     5  0.0290      0.855 0.000 0.000 0.000 0.008 0.992
#&gt; GSM49593     5  0.1668      0.847 0.032 0.000 0.000 0.028 0.940
#&gt; GSM49594     1  0.2629      0.804 0.860 0.000 0.000 0.004 0.136
#&gt; GSM49595     1  0.3456      0.803 0.800 0.000 0.000 0.016 0.184
#&gt; GSM49596     5  0.0898      0.858 0.008 0.000 0.000 0.020 0.972
#&gt; GSM49597     2  0.0290      0.789 0.008 0.992 0.000 0.000 0.000
#&gt; GSM49598     5  0.1830      0.847 0.040 0.000 0.000 0.028 0.932
#&gt; GSM49599     1  0.3086      0.820 0.816 0.000 0.000 0.004 0.180
#&gt; GSM49600     5  0.0771      0.857 0.004 0.000 0.000 0.020 0.976
#&gt; GSM49601     5  0.1668      0.848 0.032 0.000 0.000 0.028 0.940
#&gt; GSM49602     5  0.1997      0.843 0.036 0.000 0.000 0.040 0.924
#&gt; GSM49603     5  0.1997      0.843 0.036 0.000 0.000 0.040 0.924
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-4-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-5'>
<p><a id='tab-ATC-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM49613     3  0.2869      0.798 0.148 0.000 0.832 0.000 0.000 0.020
#&gt; GSM49604     4  0.3254      0.719 0.000 0.008 0.020 0.828 0.008 0.136
#&gt; GSM49605     2  0.0000      0.739 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49606     2  0.4332      0.599 0.000 0.672 0.000 0.052 0.000 0.276
#&gt; GSM49607     2  0.0146      0.738 0.000 0.996 0.004 0.000 0.000 0.000
#&gt; GSM49608     2  0.0000      0.739 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49609     2  0.6300      0.400 0.000 0.416 0.012 0.248 0.000 0.324
#&gt; GSM49610     2  0.0000      0.739 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49611     2  0.6300      0.400 0.000 0.416 0.012 0.248 0.000 0.324
#&gt; GSM49612     2  0.6300      0.400 0.000 0.416 0.012 0.248 0.000 0.324
#&gt; GSM49614     3  0.3361      0.777 0.012 0.000 0.832 0.004 0.040 0.112
#&gt; GSM49615     3  0.2869      0.798 0.148 0.000 0.832 0.000 0.000 0.020
#&gt; GSM49616     3  0.0865      0.834 0.036 0.000 0.964 0.000 0.000 0.000
#&gt; GSM49617     3  0.1585      0.817 0.012 0.000 0.940 0.000 0.036 0.012
#&gt; GSM49564     1  0.2575      0.906 0.884 0.000 0.020 0.000 0.020 0.076
#&gt; GSM49565     5  0.2146      0.635 0.116 0.000 0.000 0.000 0.880 0.004
#&gt; GSM49566     5  0.5063      0.552 0.284 0.000 0.000 0.000 0.604 0.112
#&gt; GSM49567     5  0.1364      0.606 0.048 0.000 0.000 0.004 0.944 0.004
#&gt; GSM49568     1  0.2058      0.895 0.908 0.000 0.000 0.000 0.056 0.036
#&gt; GSM49569     5  0.5066      0.555 0.276 0.000 0.000 0.000 0.608 0.116
#&gt; GSM49570     4  0.2491      0.754 0.000 0.164 0.000 0.836 0.000 0.000
#&gt; GSM49571     6  0.6221      0.469 0.000 0.000 0.012 0.212 0.360 0.416
#&gt; GSM49572     5  0.2480      0.640 0.104 0.000 0.000 0.000 0.872 0.024
#&gt; GSM49573     4  0.1267      0.812 0.000 0.060 0.000 0.940 0.000 0.000
#&gt; GSM49574     5  0.2234      0.633 0.124 0.000 0.000 0.000 0.872 0.004
#&gt; GSM49575     5  0.4556     -0.263 0.036 0.000 0.004 0.004 0.636 0.320
#&gt; GSM49576     5  0.5325      0.476 0.328 0.000 0.000 0.000 0.548 0.124
#&gt; GSM49577     5  0.1511      0.600 0.044 0.000 0.000 0.004 0.940 0.012
#&gt; GSM49578     1  0.0632      0.938 0.976 0.000 0.000 0.000 0.000 0.024
#&gt; GSM49579     5  0.4969      0.557 0.280 0.000 0.000 0.000 0.616 0.104
#&gt; GSM49580     1  0.1225      0.931 0.952 0.000 0.000 0.000 0.012 0.036
#&gt; GSM49581     1  0.0717      0.940 0.976 0.000 0.000 0.000 0.016 0.008
#&gt; GSM49582     1  0.0547      0.938 0.980 0.000 0.000 0.000 0.000 0.020
#&gt; GSM49583     2  0.0520      0.736 0.000 0.984 0.008 0.000 0.008 0.000
#&gt; GSM49584     1  0.1265      0.931 0.948 0.000 0.000 0.000 0.008 0.044
#&gt; GSM49585     1  0.1010      0.939 0.960 0.000 0.000 0.000 0.004 0.036
#&gt; GSM49586     6  0.6066      0.548 0.196 0.000 0.008 0.000 0.356 0.440
#&gt; GSM49587     1  0.0603      0.939 0.980 0.000 0.000 0.000 0.004 0.016
#&gt; GSM49588     1  0.1010      0.939 0.960 0.000 0.000 0.000 0.004 0.036
#&gt; GSM49589     1  0.2826      0.873 0.844 0.000 0.000 0.000 0.028 0.128
#&gt; GSM49590     5  0.5117      0.544 0.288 0.000 0.000 0.000 0.596 0.116
#&gt; GSM49591     1  0.0972      0.939 0.964 0.000 0.000 0.000 0.008 0.028
#&gt; GSM49592     1  0.0777      0.939 0.972 0.000 0.000 0.000 0.004 0.024
#&gt; GSM49593     1  0.1461      0.934 0.940 0.000 0.000 0.000 0.016 0.044
#&gt; GSM49594     5  0.1492      0.609 0.036 0.000 0.000 0.000 0.940 0.024
#&gt; GSM49595     5  0.1644      0.615 0.040 0.000 0.000 0.000 0.932 0.028
#&gt; GSM49596     1  0.1003      0.942 0.964 0.000 0.000 0.000 0.020 0.016
#&gt; GSM49597     2  0.0622      0.735 0.000 0.980 0.012 0.000 0.008 0.000
#&gt; GSM49598     1  0.1320      0.936 0.948 0.000 0.000 0.000 0.016 0.036
#&gt; GSM49599     5  0.1644      0.605 0.052 0.000 0.000 0.004 0.932 0.012
#&gt; GSM49600     1  0.1225      0.931 0.952 0.000 0.000 0.000 0.012 0.036
#&gt; GSM49601     1  0.1657      0.936 0.928 0.000 0.000 0.000 0.016 0.056
#&gt; GSM49602     1  0.2432      0.898 0.876 0.000 0.000 0.000 0.024 0.100
#&gt; GSM49603     1  0.2301      0.903 0.884 0.000 0.000 0.000 0.020 0.096
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-5-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-get-signatures'>
<ul>
<li><a href='#tab-ATC-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-kmeans-signature_compare](figure_cola/ATC-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-kmeans-collect-classes](figure_cola/ATC-kmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n disease.state(p) cell.type(p) k
#> ATC:kmeans 54         5.97e-07     6.72e-04 2
#> ATC:kmeans 53         3.58e-07     1.61e-13 3
#> ATC:kmeans 51         3.06e-06     9.46e-12 4
#> ATC:kmeans 46         9.79e-05     2.02e-08 5
#> ATC:kmeans 48         1.55e-04     5.11e-08 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:skmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "skmeans"]
# you can also extract it by
# res = res_list["ATC:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21168 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-skmeans-collect-plots](figure_cola/ATC-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-skmeans-select-partition-number](figure_cola/ATC-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.974       0.991          0.433 0.560   0.560
#> 3 3 0.658           0.808       0.836          0.381 0.781   0.615
#> 4 4 0.624           0.762       0.863          0.139 0.924   0.791
#> 5 5 0.613           0.630       0.821          0.077 0.896   0.700
#> 6 6 0.631           0.586       0.764          0.055 0.977   0.922
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-get-classes'>
<ul>
<li><a href='#tab-ATC-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-skmeans-get-classes-1'>
<p><a id='tab-ATC-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette   p1   p2
#&gt; GSM49613     1   0.000     1.0000 1.00 0.00
#&gt; GSM49604     2   0.000     0.9697 0.00 1.00
#&gt; GSM49605     2   0.000     0.9697 0.00 1.00
#&gt; GSM49606     2   0.000     0.9697 0.00 1.00
#&gt; GSM49607     2   0.000     0.9697 0.00 1.00
#&gt; GSM49608     2   0.000     0.9697 0.00 1.00
#&gt; GSM49609     2   0.000     0.9697 0.00 1.00
#&gt; GSM49610     2   0.000     0.9697 0.00 1.00
#&gt; GSM49611     2   0.000     0.9697 0.00 1.00
#&gt; GSM49612     2   0.000     0.9697 0.00 1.00
#&gt; GSM49614     1   0.000     1.0000 1.00 0.00
#&gt; GSM49615     1   0.000     1.0000 1.00 0.00
#&gt; GSM49616     1   0.000     1.0000 1.00 0.00
#&gt; GSM49617     1   0.000     1.0000 1.00 0.00
#&gt; GSM49564     1   0.000     1.0000 1.00 0.00
#&gt; GSM49565     1   0.000     1.0000 1.00 0.00
#&gt; GSM49566     1   0.000     1.0000 1.00 0.00
#&gt; GSM49567     1   0.000     1.0000 1.00 0.00
#&gt; GSM49568     1   0.000     1.0000 1.00 0.00
#&gt; GSM49569     1   0.000     1.0000 1.00 0.00
#&gt; GSM49570     2   0.000     0.9697 0.00 1.00
#&gt; GSM49571     2   0.000     0.9697 0.00 1.00
#&gt; GSM49572     1   0.000     1.0000 1.00 0.00
#&gt; GSM49573     2   0.000     0.9697 0.00 1.00
#&gt; GSM49574     1   0.000     1.0000 1.00 0.00
#&gt; GSM49575     2   0.000     0.9697 0.00 1.00
#&gt; GSM49576     1   0.000     1.0000 1.00 0.00
#&gt; GSM49577     2   0.000     0.9697 0.00 1.00
#&gt; GSM49578     1   0.000     1.0000 1.00 0.00
#&gt; GSM49579     1   0.000     1.0000 1.00 0.00
#&gt; GSM49580     1   0.000     1.0000 1.00 0.00
#&gt; GSM49581     1   0.000     1.0000 1.00 0.00
#&gt; GSM49582     1   0.000     1.0000 1.00 0.00
#&gt; GSM49583     2   0.000     0.9697 0.00 1.00
#&gt; GSM49584     1   0.000     1.0000 1.00 0.00
#&gt; GSM49585     1   0.000     1.0000 1.00 0.00
#&gt; GSM49586     1   0.000     1.0000 1.00 0.00
#&gt; GSM49587     1   0.000     1.0000 1.00 0.00
#&gt; GSM49588     1   0.000     1.0000 1.00 0.00
#&gt; GSM49589     1   0.000     1.0000 1.00 0.00
#&gt; GSM49590     1   0.000     1.0000 1.00 0.00
#&gt; GSM49591     1   0.000     1.0000 1.00 0.00
#&gt; GSM49592     1   0.000     1.0000 1.00 0.00
#&gt; GSM49593     1   0.000     1.0000 1.00 0.00
#&gt; GSM49594     2   0.999     0.0769 0.48 0.52
#&gt; GSM49595     1   0.000     1.0000 1.00 0.00
#&gt; GSM49596     1   0.000     1.0000 1.00 0.00
#&gt; GSM49597     2   0.000     0.9697 0.00 1.00
#&gt; GSM49598     1   0.000     1.0000 1.00 0.00
#&gt; GSM49599     1   0.000     1.0000 1.00 0.00
#&gt; GSM49600     1   0.000     1.0000 1.00 0.00
#&gt; GSM49601     1   0.000     1.0000 1.00 0.00
#&gt; GSM49602     1   0.000     1.0000 1.00 0.00
#&gt; GSM49603     1   0.000     1.0000 1.00 0.00
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-1-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-2'>
<p><a id='tab-ATC-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM49613     3  0.6280     0.8291 0.460 0.000 0.540
#&gt; GSM49604     2  0.0237     0.9400 0.000 0.996 0.004
#&gt; GSM49605     2  0.0000     0.9412 0.000 1.000 0.000
#&gt; GSM49606     2  0.0000     0.9412 0.000 1.000 0.000
#&gt; GSM49607     2  0.0000     0.9412 0.000 1.000 0.000
#&gt; GSM49608     2  0.0000     0.9412 0.000 1.000 0.000
#&gt; GSM49609     2  0.0000     0.9412 0.000 1.000 0.000
#&gt; GSM49610     2  0.0000     0.9412 0.000 1.000 0.000
#&gt; GSM49611     2  0.0000     0.9412 0.000 1.000 0.000
#&gt; GSM49612     2  0.0000     0.9412 0.000 1.000 0.000
#&gt; GSM49614     3  0.6483     0.8251 0.392 0.008 0.600
#&gt; GSM49615     3  0.6280     0.8291 0.460 0.000 0.540
#&gt; GSM49616     3  0.6225     0.8361 0.432 0.000 0.568
#&gt; GSM49617     3  0.6126     0.8315 0.400 0.000 0.600
#&gt; GSM49564     3  0.6309     0.7844 0.496 0.000 0.504
#&gt; GSM49565     1  0.3340     0.7640 0.880 0.000 0.120
#&gt; GSM49566     3  0.6307     0.7183 0.488 0.000 0.512
#&gt; GSM49567     1  0.5905     0.4575 0.648 0.000 0.352
#&gt; GSM49568     1  0.1031     0.8530 0.976 0.000 0.024
#&gt; GSM49569     3  0.6111     0.8289 0.396 0.000 0.604
#&gt; GSM49570     2  0.0237     0.9400 0.000 0.996 0.004
#&gt; GSM49571     2  0.5363     0.7653 0.000 0.724 0.276
#&gt; GSM49572     1  0.4555     0.6842 0.800 0.000 0.200
#&gt; GSM49573     2  0.0237     0.9400 0.000 0.996 0.004
#&gt; GSM49574     1  0.2261     0.8195 0.932 0.000 0.068
#&gt; GSM49575     2  0.8013     0.6042 0.072 0.564 0.364
#&gt; GSM49576     3  0.6280     0.8199 0.460 0.000 0.540
#&gt; GSM49577     2  0.6587     0.6793 0.016 0.632 0.352
#&gt; GSM49578     1  0.0000     0.8661 1.000 0.000 0.000
#&gt; GSM49579     1  0.4178     0.6519 0.828 0.000 0.172
#&gt; GSM49580     1  0.0592     0.8604 0.988 0.000 0.012
#&gt; GSM49581     1  0.0892     0.8595 0.980 0.000 0.020
#&gt; GSM49582     1  0.0000     0.8661 1.000 0.000 0.000
#&gt; GSM49583     2  0.0000     0.9412 0.000 1.000 0.000
#&gt; GSM49584     1  0.2537     0.7588 0.920 0.000 0.080
#&gt; GSM49585     1  0.0000     0.8661 1.000 0.000 0.000
#&gt; GSM49586     1  0.5465     0.4977 0.712 0.000 0.288
#&gt; GSM49587     1  0.0424     0.8637 0.992 0.000 0.008
#&gt; GSM49588     1  0.0000     0.8661 1.000 0.000 0.000
#&gt; GSM49589     3  0.6295     0.8186 0.472 0.000 0.528
#&gt; GSM49590     3  0.6154     0.8345 0.408 0.000 0.592
#&gt; GSM49591     1  0.0237     0.8661 0.996 0.000 0.004
#&gt; GSM49592     1  0.0000     0.8661 1.000 0.000 0.000
#&gt; GSM49593     1  0.0424     0.8638 0.992 0.000 0.008
#&gt; GSM49594     3  0.8772    -0.0908 0.364 0.120 0.516
#&gt; GSM49595     1  0.5882     0.4640 0.652 0.000 0.348
#&gt; GSM49596     1  0.0424     0.8655 0.992 0.000 0.008
#&gt; GSM49597     2  0.0000     0.9412 0.000 1.000 0.000
#&gt; GSM49598     1  0.0892     0.8626 0.980 0.000 0.020
#&gt; GSM49599     1  0.4121     0.7112 0.832 0.000 0.168
#&gt; GSM49600     1  0.0592     0.8615 0.988 0.000 0.012
#&gt; GSM49601     1  0.0592     0.8617 0.988 0.000 0.012
#&gt; GSM49602     1  0.0892     0.8559 0.980 0.000 0.020
#&gt; GSM49603     1  0.0892     0.8559 0.980 0.000 0.020
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-2-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-3'>
<p><a id='tab-ATC-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM49613     3  0.3837      0.747 0.224 0.000 0.776 0.000
#&gt; GSM49604     2  0.2589      0.876 0.000 0.884 0.000 0.116
#&gt; GSM49605     2  0.0000      0.966 0.000 1.000 0.000 0.000
#&gt; GSM49606     2  0.0000      0.966 0.000 1.000 0.000 0.000
#&gt; GSM49607     2  0.0000      0.966 0.000 1.000 0.000 0.000
#&gt; GSM49608     2  0.0000      0.966 0.000 1.000 0.000 0.000
#&gt; GSM49609     2  0.0000      0.966 0.000 1.000 0.000 0.000
#&gt; GSM49610     2  0.0000      0.966 0.000 1.000 0.000 0.000
#&gt; GSM49611     2  0.0000      0.966 0.000 1.000 0.000 0.000
#&gt; GSM49612     2  0.0000      0.966 0.000 1.000 0.000 0.000
#&gt; GSM49614     3  0.1557      0.637 0.056 0.000 0.944 0.000
#&gt; GSM49615     3  0.3837      0.747 0.224 0.000 0.776 0.000
#&gt; GSM49616     3  0.2281      0.676 0.096 0.000 0.904 0.000
#&gt; GSM49617     3  0.1637      0.643 0.060 0.000 0.940 0.000
#&gt; GSM49564     3  0.4898      0.610 0.416 0.000 0.584 0.000
#&gt; GSM49565     1  0.4323      0.716 0.788 0.000 0.028 0.184
#&gt; GSM49566     3  0.5784      0.540 0.412 0.000 0.556 0.032
#&gt; GSM49567     1  0.6458      0.150 0.520 0.000 0.072 0.408
#&gt; GSM49568     1  0.1902      0.848 0.932 0.000 0.064 0.004
#&gt; GSM49569     3  0.4245      0.710 0.196 0.000 0.784 0.020
#&gt; GSM49570     2  0.2530      0.880 0.000 0.888 0.000 0.112
#&gt; GSM49571     4  0.4382      0.407 0.000 0.296 0.000 0.704
#&gt; GSM49572     1  0.5977      0.576 0.680 0.000 0.104 0.216
#&gt; GSM49573     2  0.2647      0.872 0.000 0.880 0.000 0.120
#&gt; GSM49574     1  0.2909      0.820 0.888 0.000 0.020 0.092
#&gt; GSM49575     4  0.3037      0.615 0.020 0.100 0.000 0.880
#&gt; GSM49576     3  0.5821      0.535 0.432 0.000 0.536 0.032
#&gt; GSM49577     4  0.6554      0.404 0.008 0.336 0.072 0.584
#&gt; GSM49578     1  0.0000      0.875 1.000 0.000 0.000 0.000
#&gt; GSM49579     1  0.5705      0.596 0.712 0.000 0.180 0.108
#&gt; GSM49580     1  0.1118      0.868 0.964 0.000 0.036 0.000
#&gt; GSM49581     1  0.1022      0.867 0.968 0.000 0.000 0.032
#&gt; GSM49582     1  0.0000      0.875 1.000 0.000 0.000 0.000
#&gt; GSM49583     2  0.0000      0.966 0.000 1.000 0.000 0.000
#&gt; GSM49584     1  0.2469      0.796 0.892 0.000 0.108 0.000
#&gt; GSM49585     1  0.0188      0.875 0.996 0.000 0.000 0.004
#&gt; GSM49586     4  0.4040      0.487 0.248 0.000 0.000 0.752
#&gt; GSM49587     1  0.0707      0.873 0.980 0.000 0.020 0.000
#&gt; GSM49588     1  0.0188      0.875 0.996 0.000 0.000 0.004
#&gt; GSM49589     3  0.4889      0.697 0.360 0.000 0.636 0.004
#&gt; GSM49590     3  0.5578      0.698 0.312 0.000 0.648 0.040
#&gt; GSM49591     1  0.0336      0.876 0.992 0.000 0.000 0.008
#&gt; GSM49592     1  0.0000      0.875 1.000 0.000 0.000 0.000
#&gt; GSM49593     1  0.1388      0.868 0.960 0.000 0.012 0.028
#&gt; GSM49594     4  0.5655      0.563 0.056 0.052 0.128 0.764
#&gt; GSM49595     4  0.7156      0.111 0.388 0.000 0.136 0.476
#&gt; GSM49596     1  0.0657      0.877 0.984 0.000 0.004 0.012
#&gt; GSM49597     2  0.0000      0.966 0.000 1.000 0.000 0.000
#&gt; GSM49598     1  0.1938      0.859 0.936 0.000 0.012 0.052
#&gt; GSM49599     1  0.4853      0.655 0.744 0.000 0.036 0.220
#&gt; GSM49600     1  0.1389      0.862 0.952 0.000 0.048 0.000
#&gt; GSM49601     1  0.1256      0.870 0.964 0.000 0.008 0.028
#&gt; GSM49602     1  0.2399      0.849 0.920 0.000 0.032 0.048
#&gt; GSM49603     1  0.2124      0.855 0.932 0.000 0.028 0.040
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-3-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-4'>
<p><a id='tab-ATC-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM49613     3  0.3480     0.6518 0.248 0.000 0.752 0.000 0.000
#&gt; GSM49604     2  0.3636     0.6659 0.000 0.728 0.000 0.272 0.000
#&gt; GSM49605     2  0.0000     0.9290 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49606     2  0.0000     0.9290 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49607     2  0.0000     0.9290 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49608     2  0.0000     0.9290 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49609     2  0.0000     0.9290 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49610     2  0.0000     0.9290 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49611     2  0.0000     0.9290 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49612     2  0.0000     0.9290 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49614     3  0.0162     0.5913 0.004 0.000 0.996 0.000 0.000
#&gt; GSM49615     3  0.3508     0.6495 0.252 0.000 0.748 0.000 0.000
#&gt; GSM49616     3  0.1341     0.6318 0.056 0.000 0.944 0.000 0.000
#&gt; GSM49617     3  0.0609     0.6082 0.020 0.000 0.980 0.000 0.000
#&gt; GSM49564     1  0.5014    -0.0569 0.536 0.000 0.432 0.000 0.032
#&gt; GSM49565     1  0.4557     0.0840 0.552 0.000 0.004 0.004 0.440
#&gt; GSM49566     1  0.7031    -0.2079 0.372 0.000 0.328 0.008 0.292
#&gt; GSM49567     5  0.5545     0.5001 0.212 0.000 0.016 0.100 0.672
#&gt; GSM49568     1  0.2899     0.7405 0.872 0.000 0.028 0.004 0.096
#&gt; GSM49569     3  0.5934     0.5108 0.156 0.000 0.648 0.020 0.176
#&gt; GSM49570     2  0.3177     0.7461 0.000 0.792 0.000 0.208 0.000
#&gt; GSM49571     4  0.2074     0.7911 0.000 0.104 0.000 0.896 0.000
#&gt; GSM49572     5  0.5078     0.4112 0.336 0.000 0.020 0.020 0.624
#&gt; GSM49573     2  0.3707     0.6467 0.000 0.716 0.000 0.284 0.000
#&gt; GSM49574     1  0.4348     0.6262 0.768 0.000 0.020 0.032 0.180
#&gt; GSM49575     4  0.1862     0.8153 0.004 0.016 0.000 0.932 0.048
#&gt; GSM49576     1  0.7015    -0.1511 0.428 0.000 0.336 0.016 0.220
#&gt; GSM49577     5  0.5799     0.2949 0.004 0.160 0.008 0.172 0.656
#&gt; GSM49578     1  0.0510     0.7698 0.984 0.000 0.000 0.000 0.016
#&gt; GSM49579     1  0.5723     0.1502 0.532 0.000 0.076 0.004 0.388
#&gt; GSM49580     1  0.2206     0.7593 0.912 0.000 0.016 0.004 0.068
#&gt; GSM49581     1  0.2922     0.7467 0.872 0.000 0.000 0.072 0.056
#&gt; GSM49582     1  0.0703     0.7690 0.976 0.000 0.000 0.000 0.024
#&gt; GSM49583     2  0.0000     0.9290 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49584     1  0.2954     0.7381 0.876 0.000 0.056 0.004 0.064
#&gt; GSM49585     1  0.0932     0.7727 0.972 0.000 0.004 0.004 0.020
#&gt; GSM49586     4  0.2388     0.7794 0.072 0.000 0.000 0.900 0.028
#&gt; GSM49587     1  0.0865     0.7719 0.972 0.000 0.004 0.000 0.024
#&gt; GSM49588     1  0.0671     0.7725 0.980 0.000 0.004 0.000 0.016
#&gt; GSM49589     3  0.5431     0.2805 0.448 0.000 0.500 0.004 0.048
#&gt; GSM49590     3  0.6670     0.3707 0.316 0.000 0.464 0.004 0.216
#&gt; GSM49591     1  0.0898     0.7731 0.972 0.000 0.000 0.008 0.020
#&gt; GSM49592     1  0.0566     0.7710 0.984 0.000 0.004 0.000 0.012
#&gt; GSM49593     1  0.2517     0.7502 0.884 0.000 0.004 0.008 0.104
#&gt; GSM49594     5  0.6246     0.3701 0.036 0.032 0.056 0.224 0.652
#&gt; GSM49595     5  0.6380     0.4403 0.160 0.000 0.052 0.156 0.632
#&gt; GSM49596     1  0.1608     0.7717 0.928 0.000 0.000 0.000 0.072
#&gt; GSM49597     2  0.0000     0.9290 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49598     1  0.3178     0.7428 0.860 0.000 0.004 0.048 0.088
#&gt; GSM49599     1  0.7278    -0.0874 0.444 0.000 0.052 0.156 0.348
#&gt; GSM49600     1  0.3037     0.7543 0.864 0.000 0.032 0.004 0.100
#&gt; GSM49601     1  0.2284     0.7566 0.896 0.000 0.004 0.004 0.096
#&gt; GSM49602     1  0.3870     0.7143 0.816 0.000 0.024 0.028 0.132
#&gt; GSM49603     1  0.3458     0.7258 0.840 0.000 0.016 0.024 0.120
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-4-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-5'>
<p><a id='tab-ATC-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM49613     3  0.3680     0.5937 0.232 0.000 0.744 0.000 0.004 0.020
#&gt; GSM49604     2  0.3819     0.4974 0.000 0.624 0.000 0.372 0.000 0.004
#&gt; GSM49605     2  0.0000     0.9034 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49606     2  0.0000     0.9034 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49607     2  0.0000     0.9034 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49608     2  0.0000     0.9034 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49609     2  0.0146     0.9022 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; GSM49610     2  0.0000     0.9034 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49611     2  0.0146     0.9022 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; GSM49612     2  0.0146     0.9022 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; GSM49614     3  0.0837     0.5380 0.004 0.000 0.972 0.000 0.020 0.004
#&gt; GSM49615     3  0.3679     0.5835 0.260 0.000 0.724 0.000 0.004 0.012
#&gt; GSM49616     3  0.0790     0.5704 0.032 0.000 0.968 0.000 0.000 0.000
#&gt; GSM49617     3  0.0622     0.5505 0.008 0.000 0.980 0.000 0.012 0.000
#&gt; GSM49564     1  0.5808     0.2102 0.540 0.000 0.344 0.008 0.028 0.080
#&gt; GSM49565     1  0.6191    -0.2147 0.448 0.000 0.004 0.004 0.300 0.244
#&gt; GSM49566     1  0.7483    -0.1070 0.320 0.000 0.244 0.004 0.112 0.320
#&gt; GSM49567     6  0.5862     0.3952 0.128 0.000 0.012 0.048 0.172 0.640
#&gt; GSM49568     1  0.4191     0.6461 0.760 0.000 0.028 0.000 0.048 0.164
#&gt; GSM49569     3  0.7461     0.2332 0.140 0.000 0.392 0.004 0.196 0.268
#&gt; GSM49570     2  0.3468     0.6305 0.000 0.712 0.000 0.284 0.000 0.004
#&gt; GSM49571     4  0.0777     0.8837 0.000 0.024 0.000 0.972 0.004 0.000
#&gt; GSM49572     6  0.5668     0.4289 0.240 0.000 0.024 0.004 0.124 0.608
#&gt; GSM49573     2  0.3881     0.4488 0.000 0.600 0.000 0.396 0.000 0.004
#&gt; GSM49574     1  0.6444     0.1698 0.528 0.000 0.016 0.024 0.208 0.224
#&gt; GSM49575     4  0.2677     0.8755 0.000 0.016 0.000 0.876 0.024 0.084
#&gt; GSM49576     1  0.7327    -0.0284 0.412 0.000 0.284 0.004 0.164 0.136
#&gt; GSM49577     6  0.6256    -0.0766 0.004 0.080 0.004 0.052 0.384 0.476
#&gt; GSM49578     1  0.1230     0.7012 0.956 0.000 0.008 0.000 0.008 0.028
#&gt; GSM49579     1  0.6911    -0.0822 0.412 0.000 0.076 0.000 0.184 0.328
#&gt; GSM49580     1  0.3424     0.6824 0.816 0.000 0.016 0.000 0.032 0.136
#&gt; GSM49581     1  0.4208     0.6578 0.768 0.000 0.000 0.064 0.028 0.140
#&gt; GSM49582     1  0.1471     0.6987 0.932 0.000 0.000 0.000 0.004 0.064
#&gt; GSM49583     2  0.0000     0.9034 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49584     1  0.3539     0.6726 0.808 0.000 0.044 0.000 0.012 0.136
#&gt; GSM49585     1  0.2520     0.6947 0.896 0.000 0.012 0.012 0.020 0.060
#&gt; GSM49586     4  0.1974     0.8724 0.012 0.000 0.000 0.920 0.020 0.048
#&gt; GSM49587     1  0.2414     0.7003 0.896 0.000 0.012 0.000 0.036 0.056
#&gt; GSM49588     1  0.1628     0.7020 0.940 0.000 0.012 0.004 0.008 0.036
#&gt; GSM49589     3  0.5843     0.2684 0.388 0.000 0.504 0.008 0.036 0.064
#&gt; GSM49590     3  0.7688     0.2398 0.272 0.000 0.360 0.008 0.172 0.188
#&gt; GSM49591     1  0.2514     0.6978 0.896 0.000 0.004 0.016 0.032 0.052
#&gt; GSM49592     1  0.1565     0.6994 0.944 0.000 0.008 0.008 0.008 0.032
#&gt; GSM49593     1  0.3883     0.6927 0.792 0.000 0.004 0.008 0.076 0.120
#&gt; GSM49594     5  0.2821     0.8654 0.008 0.000 0.020 0.064 0.880 0.028
#&gt; GSM49595     5  0.2895     0.8640 0.044 0.000 0.020 0.044 0.880 0.012
#&gt; GSM49596     1  0.3565     0.6976 0.820 0.000 0.008 0.004 0.076 0.092
#&gt; GSM49597     2  0.0000     0.9034 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49598     1  0.5173     0.6653 0.700 0.000 0.004 0.048 0.092 0.156
#&gt; GSM49599     6  0.7740     0.2078 0.272 0.000 0.036 0.104 0.184 0.404
#&gt; GSM49600     1  0.5011     0.6543 0.712 0.000 0.060 0.008 0.048 0.172
#&gt; GSM49601     1  0.3739     0.6876 0.812 0.000 0.012 0.008 0.060 0.108
#&gt; GSM49602     1  0.5478     0.6074 0.660 0.000 0.012 0.020 0.148 0.160
#&gt; GSM49603     1  0.4871     0.6480 0.720 0.000 0.008 0.020 0.108 0.144
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-5-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-get-signatures'>
<ul>
<li><a href='#tab-ATC-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-skmeans-signature_compare](figure_cola/ATC-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-skmeans-collect-classes](figure_cola/ATC-skmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>              n disease.state(p) cell.type(p) k
#> ATC:skmeans 53         2.77e-05     3.04e-03 2
#> ATC:skmeans 50         5.25e-05     1.11e-05 3
#> ATC:skmeans 49         8.04e-06     3.00e-05 4
#> ATC:skmeans 42         1.88e-04     7.59e-07 5
#> ATC:skmeans 39         1.83e-04     4.32e-08 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:pam**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "pam"]
# you can also extract it by
# res = res_list["ATC:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21168 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-pam-collect-plots](figure_cola/ATC-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-pam-select-partition-number](figure_cola/ATC-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.999       0.999          0.372 0.628   0.628
#> 3 3 0.573           0.634       0.847          0.378 0.945   0.913
#> 4 4 0.582           0.635       0.798          0.129 0.876   0.793
#> 5 5 0.539           0.605       0.774          0.182 0.742   0.497
#> 6 6 0.721           0.732       0.885          0.104 0.869   0.587
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-get-classes'>
<ul>
<li><a href='#tab-ATC-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-pam-get-classes-1'>
<p><a id='tab-ATC-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM49613     1   0.000      1.000 1.000 0.000
#&gt; GSM49604     2   0.000      0.997 0.000 1.000
#&gt; GSM49605     2   0.000      0.997 0.000 1.000
#&gt; GSM49606     2   0.000      0.997 0.000 1.000
#&gt; GSM49607     2   0.000      0.997 0.000 1.000
#&gt; GSM49608     2   0.000      0.997 0.000 1.000
#&gt; GSM49609     2   0.224      0.963 0.036 0.964
#&gt; GSM49610     2   0.000      0.997 0.000 1.000
#&gt; GSM49611     2   0.000      0.997 0.000 1.000
#&gt; GSM49612     2   0.000      0.997 0.000 1.000
#&gt; GSM49614     1   0.000      1.000 1.000 0.000
#&gt; GSM49615     1   0.000      1.000 1.000 0.000
#&gt; GSM49616     1   0.000      1.000 1.000 0.000
#&gt; GSM49617     1   0.000      1.000 1.000 0.000
#&gt; GSM49564     1   0.000      1.000 1.000 0.000
#&gt; GSM49565     1   0.000      1.000 1.000 0.000
#&gt; GSM49566     1   0.000      1.000 1.000 0.000
#&gt; GSM49567     1   0.000      1.000 1.000 0.000
#&gt; GSM49568     1   0.000      1.000 1.000 0.000
#&gt; GSM49569     1   0.000      1.000 1.000 0.000
#&gt; GSM49570     2   0.000      0.997 0.000 1.000
#&gt; GSM49571     1   0.000      1.000 1.000 0.000
#&gt; GSM49572     1   0.000      1.000 1.000 0.000
#&gt; GSM49573     2   0.000      0.997 0.000 1.000
#&gt; GSM49574     1   0.000      1.000 1.000 0.000
#&gt; GSM49575     1   0.000      1.000 1.000 0.000
#&gt; GSM49576     1   0.000      1.000 1.000 0.000
#&gt; GSM49577     1   0.000      1.000 1.000 0.000
#&gt; GSM49578     1   0.000      1.000 1.000 0.000
#&gt; GSM49579     1   0.000      1.000 1.000 0.000
#&gt; GSM49580     1   0.000      1.000 1.000 0.000
#&gt; GSM49581     1   0.000      1.000 1.000 0.000
#&gt; GSM49582     1   0.000      1.000 1.000 0.000
#&gt; GSM49583     2   0.000      0.997 0.000 1.000
#&gt; GSM49584     1   0.000      1.000 1.000 0.000
#&gt; GSM49585     1   0.000      1.000 1.000 0.000
#&gt; GSM49586     1   0.000      1.000 1.000 0.000
#&gt; GSM49587     1   0.000      1.000 1.000 0.000
#&gt; GSM49588     1   0.000      1.000 1.000 0.000
#&gt; GSM49589     1   0.000      1.000 1.000 0.000
#&gt; GSM49590     1   0.000      1.000 1.000 0.000
#&gt; GSM49591     1   0.000      1.000 1.000 0.000
#&gt; GSM49592     1   0.000      1.000 1.000 0.000
#&gt; GSM49593     1   0.000      1.000 1.000 0.000
#&gt; GSM49594     1   0.000      1.000 1.000 0.000
#&gt; GSM49595     1   0.000      1.000 1.000 0.000
#&gt; GSM49596     1   0.000      1.000 1.000 0.000
#&gt; GSM49597     2   0.000      0.997 0.000 1.000
#&gt; GSM49598     1   0.000      1.000 1.000 0.000
#&gt; GSM49599     1   0.000      1.000 1.000 0.000
#&gt; GSM49600     1   0.000      1.000 1.000 0.000
#&gt; GSM49601     1   0.000      1.000 1.000 0.000
#&gt; GSM49602     1   0.000      1.000 1.000 0.000
#&gt; GSM49603     1   0.000      1.000 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-1-a').click(function(){
  $('#tab-ATC-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-2'>
<p><a id='tab-ATC-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM49613     1  0.5948     -0.271 0.640 0.000 0.360
#&gt; GSM49604     2  0.5560      0.809 0.000 0.700 0.300
#&gt; GSM49605     2  0.0000      0.905 0.000 1.000 0.000
#&gt; GSM49606     2  0.0000      0.905 0.000 1.000 0.000
#&gt; GSM49607     2  0.0000      0.905 0.000 1.000 0.000
#&gt; GSM49608     2  0.0000      0.905 0.000 1.000 0.000
#&gt; GSM49609     2  0.6299      0.452 0.000 0.524 0.476
#&gt; GSM49610     2  0.0000      0.905 0.000 1.000 0.000
#&gt; GSM49611     2  0.3879      0.854 0.000 0.848 0.152
#&gt; GSM49612     2  0.5058      0.797 0.000 0.756 0.244
#&gt; GSM49614     3  0.5591      0.808 0.304 0.000 0.696
#&gt; GSM49615     1  0.5948     -0.271 0.640 0.000 0.360
#&gt; GSM49616     1  0.5948     -0.271 0.640 0.000 0.360
#&gt; GSM49617     3  0.6095      0.783 0.392 0.000 0.608
#&gt; GSM49564     1  0.0000      0.728 1.000 0.000 0.000
#&gt; GSM49565     1  0.5216      0.515 0.740 0.000 0.260
#&gt; GSM49566     1  0.3619      0.653 0.864 0.000 0.136
#&gt; GSM49567     1  0.5431      0.465 0.716 0.000 0.284
#&gt; GSM49568     1  0.0592      0.728 0.988 0.000 0.012
#&gt; GSM49569     1  0.5810      0.393 0.664 0.000 0.336
#&gt; GSM49570     2  0.3879      0.854 0.000 0.848 0.152
#&gt; GSM49571     1  0.5810      0.393 0.664 0.000 0.336
#&gt; GSM49572     1  0.4002      0.629 0.840 0.000 0.160
#&gt; GSM49573     2  0.3879      0.854 0.000 0.848 0.152
#&gt; GSM49574     1  0.3551      0.654 0.868 0.000 0.132
#&gt; GSM49575     1  0.5810      0.393 0.664 0.000 0.336
#&gt; GSM49576     1  0.5216      0.523 0.740 0.000 0.260
#&gt; GSM49577     1  0.5810      0.393 0.664 0.000 0.336
#&gt; GSM49578     1  0.0000      0.728 1.000 0.000 0.000
#&gt; GSM49579     1  0.5785      0.401 0.668 0.000 0.332
#&gt; GSM49580     1  0.0237      0.728 0.996 0.000 0.004
#&gt; GSM49581     1  0.0000      0.728 1.000 0.000 0.000
#&gt; GSM49582     1  0.0000      0.728 1.000 0.000 0.000
#&gt; GSM49583     2  0.0000      0.905 0.000 1.000 0.000
#&gt; GSM49584     1  0.0424      0.728 0.992 0.000 0.008
#&gt; GSM49585     1  0.0000      0.728 1.000 0.000 0.000
#&gt; GSM49586     1  0.3482      0.658 0.872 0.000 0.128
#&gt; GSM49587     1  0.0000      0.728 1.000 0.000 0.000
#&gt; GSM49588     1  0.0000      0.728 1.000 0.000 0.000
#&gt; GSM49589     1  0.2066      0.710 0.940 0.000 0.060
#&gt; GSM49590     1  0.3619      0.655 0.864 0.000 0.136
#&gt; GSM49591     1  0.0000      0.728 1.000 0.000 0.000
#&gt; GSM49592     1  0.0000      0.728 1.000 0.000 0.000
#&gt; GSM49593     1  0.0592      0.728 0.988 0.000 0.012
#&gt; GSM49594     1  0.5810      0.393 0.664 0.000 0.336
#&gt; GSM49595     1  0.5810      0.393 0.664 0.000 0.336
#&gt; GSM49596     1  0.0747      0.728 0.984 0.000 0.016
#&gt; GSM49597     2  0.0000      0.905 0.000 1.000 0.000
#&gt; GSM49598     1  0.0592      0.728 0.988 0.000 0.012
#&gt; GSM49599     1  0.5216      0.523 0.740 0.000 0.260
#&gt; GSM49600     1  0.0000      0.728 1.000 0.000 0.000
#&gt; GSM49601     1  0.0424      0.728 0.992 0.000 0.008
#&gt; GSM49602     1  0.3686      0.647 0.860 0.000 0.140
#&gt; GSM49603     1  0.0592      0.728 0.988 0.000 0.012
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-2-a').click(function(){
  $('#tab-ATC-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-3'>
<p><a id='tab-ATC-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM49613     3  0.2081      0.768 0.084 0.000 0.916 0.000
#&gt; GSM49604     4  0.1474      0.552 0.000 0.052 0.000 0.948
#&gt; GSM49605     2  0.0000      0.825 0.000 1.000 0.000 0.000
#&gt; GSM49606     2  0.0000      0.825 0.000 1.000 0.000 0.000
#&gt; GSM49607     2  0.0000      0.825 0.000 1.000 0.000 0.000
#&gt; GSM49608     2  0.0000      0.825 0.000 1.000 0.000 0.000
#&gt; GSM49609     2  0.7148      0.382 0.128 0.524 0.004 0.344
#&gt; GSM49610     2  0.0000      0.825 0.000 1.000 0.000 0.000
#&gt; GSM49611     2  0.4819      0.556 0.000 0.652 0.004 0.344
#&gt; GSM49612     2  0.5464      0.539 0.020 0.632 0.004 0.344
#&gt; GSM49614     1  0.4999     -0.509 0.508 0.000 0.492 0.000
#&gt; GSM49615     3  0.1867      0.772 0.072 0.000 0.928 0.000
#&gt; GSM49616     3  0.1867      0.772 0.072 0.000 0.928 0.000
#&gt; GSM49617     3  0.4981      0.273 0.464 0.000 0.536 0.000
#&gt; GSM49564     1  0.4888      0.676 0.588 0.000 0.412 0.000
#&gt; GSM49565     1  0.1389      0.574 0.952 0.000 0.048 0.000
#&gt; GSM49566     1  0.3726      0.634 0.788 0.000 0.212 0.000
#&gt; GSM49567     1  0.2011      0.594 0.920 0.000 0.080 0.000
#&gt; GSM49568     1  0.4907      0.676 0.580 0.000 0.420 0.000
#&gt; GSM49569     1  0.0000      0.564 1.000 0.000 0.000 0.000
#&gt; GSM49570     4  0.5877      0.767 0.000 0.276 0.068 0.656
#&gt; GSM49571     1  0.0000      0.564 1.000 0.000 0.000 0.000
#&gt; GSM49572     1  0.3486      0.623 0.812 0.000 0.188 0.000
#&gt; GSM49573     4  0.5877      0.767 0.000 0.276 0.068 0.656
#&gt; GSM49574     1  0.3873      0.644 0.772 0.000 0.228 0.000
#&gt; GSM49575     1  0.0000      0.564 1.000 0.000 0.000 0.000
#&gt; GSM49576     1  0.0188      0.567 0.996 0.000 0.004 0.000
#&gt; GSM49577     1  0.0000      0.564 1.000 0.000 0.000 0.000
#&gt; GSM49578     1  0.4941      0.670 0.564 0.000 0.436 0.000
#&gt; GSM49579     1  0.0336      0.569 0.992 0.000 0.008 0.000
#&gt; GSM49580     1  0.4933      0.672 0.568 0.000 0.432 0.000
#&gt; GSM49581     1  0.4941      0.670 0.564 0.000 0.436 0.000
#&gt; GSM49582     1  0.4941      0.670 0.564 0.000 0.436 0.000
#&gt; GSM49583     2  0.0000      0.825 0.000 1.000 0.000 0.000
#&gt; GSM49584     1  0.4925      0.673 0.572 0.000 0.428 0.000
#&gt; GSM49585     1  0.4941      0.670 0.564 0.000 0.436 0.000
#&gt; GSM49586     1  0.3801      0.642 0.780 0.000 0.220 0.000
#&gt; GSM49587     1  0.4941      0.670 0.564 0.000 0.436 0.000
#&gt; GSM49588     1  0.4941      0.670 0.564 0.000 0.436 0.000
#&gt; GSM49589     1  0.4543      0.675 0.676 0.000 0.324 0.000
#&gt; GSM49590     1  0.3266      0.633 0.832 0.000 0.168 0.000
#&gt; GSM49591     1  0.4941      0.670 0.564 0.000 0.436 0.000
#&gt; GSM49592     1  0.4941      0.670 0.564 0.000 0.436 0.000
#&gt; GSM49593     1  0.4761      0.680 0.628 0.000 0.372 0.000
#&gt; GSM49594     1  0.0000      0.564 1.000 0.000 0.000 0.000
#&gt; GSM49595     1  0.0000      0.564 1.000 0.000 0.000 0.000
#&gt; GSM49596     1  0.4866      0.680 0.596 0.000 0.404 0.000
#&gt; GSM49597     2  0.0000      0.825 0.000 1.000 0.000 0.000
#&gt; GSM49598     1  0.4761      0.680 0.628 0.000 0.372 0.000
#&gt; GSM49599     1  0.0921      0.579 0.972 0.000 0.028 0.000
#&gt; GSM49600     1  0.4941      0.670 0.564 0.000 0.436 0.000
#&gt; GSM49601     1  0.4898      0.675 0.584 0.000 0.416 0.000
#&gt; GSM49602     1  0.3649      0.637 0.796 0.000 0.204 0.000
#&gt; GSM49603     1  0.4761      0.680 0.628 0.000 0.372 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-3-a').click(function(){
  $('#tab-ATC-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-4'>
<p><a id='tab-ATC-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM49613     3  0.4668      0.664 0.352 0.000 0.624 0.000 0.024
#&gt; GSM49604     4  0.2930      0.500 0.000 0.032 0.032 0.888 0.048
#&gt; GSM49605     2  0.0000      0.995 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49606     2  0.0703      0.968 0.000 0.976 0.000 0.024 0.000
#&gt; GSM49607     2  0.0000      0.995 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49608     2  0.0000      0.995 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49609     4  0.6071      0.328 0.000 0.300 0.000 0.548 0.152
#&gt; GSM49610     2  0.0000      0.995 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49611     4  0.4278      0.154 0.000 0.452 0.000 0.548 0.000
#&gt; GSM49612     4  0.5281      0.255 0.000 0.400 0.000 0.548 0.052
#&gt; GSM49614     3  0.4114      0.434 0.000 0.000 0.624 0.000 0.376
#&gt; GSM49615     3  0.4114      0.661 0.376 0.000 0.624 0.000 0.000
#&gt; GSM49616     3  0.4114      0.661 0.376 0.000 0.624 0.000 0.000
#&gt; GSM49617     3  0.4794      0.474 0.032 0.000 0.624 0.000 0.344
#&gt; GSM49564     1  0.3366      0.547 0.768 0.000 0.000 0.000 0.232
#&gt; GSM49565     5  0.4268      0.420 0.444 0.000 0.000 0.000 0.556
#&gt; GSM49566     1  0.3949      0.260 0.668 0.000 0.000 0.000 0.332
#&gt; GSM49567     1  0.4294     -0.276 0.532 0.000 0.000 0.000 0.468
#&gt; GSM49568     1  0.1410      0.718 0.940 0.000 0.000 0.000 0.060
#&gt; GSM49569     5  0.3109      0.777 0.200 0.000 0.000 0.000 0.800
#&gt; GSM49570     4  0.6396      0.401 0.000 0.000 0.376 0.452 0.172
#&gt; GSM49571     5  0.3242      0.766 0.216 0.000 0.000 0.000 0.784
#&gt; GSM49572     1  0.3816      0.269 0.696 0.000 0.000 0.000 0.304
#&gt; GSM49573     4  0.6396      0.401 0.000 0.000 0.376 0.452 0.172
#&gt; GSM49574     1  0.3999      0.145 0.656 0.000 0.000 0.000 0.344
#&gt; GSM49575     5  0.2852      0.787 0.172 0.000 0.000 0.000 0.828
#&gt; GSM49576     5  0.2852      0.787 0.172 0.000 0.000 0.000 0.828
#&gt; GSM49577     5  0.2852      0.787 0.172 0.000 0.000 0.000 0.828
#&gt; GSM49578     1  0.0000      0.752 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49579     5  0.3395      0.745 0.236 0.000 0.000 0.000 0.764
#&gt; GSM49580     1  0.0609      0.743 0.980 0.000 0.000 0.000 0.020
#&gt; GSM49581     1  0.0000      0.752 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49582     1  0.0000      0.752 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49583     2  0.0000      0.995 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49584     1  0.1270      0.722 0.948 0.000 0.000 0.000 0.052
#&gt; GSM49585     1  0.0000      0.752 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49586     5  0.4273      0.336 0.448 0.000 0.000 0.000 0.552
#&gt; GSM49587     1  0.0000      0.752 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49588     1  0.0000      0.752 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49589     1  0.3999      0.354 0.656 0.000 0.000 0.000 0.344
#&gt; GSM49590     5  0.4242      0.333 0.428 0.000 0.000 0.000 0.572
#&gt; GSM49591     1  0.0000      0.752 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49592     1  0.0000      0.752 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49593     1  0.3837      0.427 0.692 0.000 0.000 0.000 0.308
#&gt; GSM49594     5  0.2852      0.787 0.172 0.000 0.000 0.000 0.828
#&gt; GSM49595     5  0.2891      0.787 0.176 0.000 0.000 0.000 0.824
#&gt; GSM49596     1  0.1851      0.708 0.912 0.000 0.000 0.000 0.088
#&gt; GSM49597     2  0.0000      0.995 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49598     1  0.4088      0.278 0.632 0.000 0.000 0.000 0.368
#&gt; GSM49599     5  0.4242      0.469 0.428 0.000 0.000 0.000 0.572
#&gt; GSM49600     1  0.0000      0.752 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49601     1  0.3305      0.557 0.776 0.000 0.000 0.000 0.224
#&gt; GSM49602     5  0.4249      0.381 0.432 0.000 0.000 0.000 0.568
#&gt; GSM49603     1  0.3837      0.427 0.692 0.000 0.000 0.000 0.308
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-4-a').click(function(){
  $('#tab-ATC-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-5'>
<p><a id='tab-ATC-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3  p4    p5    p6
#&gt; GSM49613     3  0.0146      0.996 0.004 0.000 0.996 0.0 0.000 0.000
#&gt; GSM49604     6  0.3409      0.570 0.000 0.000 0.000 0.3 0.000 0.700
#&gt; GSM49605     2  0.0000      0.983 0.000 1.000 0.000 0.0 0.000 0.000
#&gt; GSM49606     2  0.1765      0.892 0.000 0.904 0.000 0.0 0.000 0.096
#&gt; GSM49607     2  0.0000      0.983 0.000 1.000 0.000 0.0 0.000 0.000
#&gt; GSM49608     2  0.0000      0.983 0.000 1.000 0.000 0.0 0.000 0.000
#&gt; GSM49609     6  0.0000      0.892 0.000 0.000 0.000 0.0 0.000 1.000
#&gt; GSM49610     2  0.0000      0.983 0.000 1.000 0.000 0.0 0.000 0.000
#&gt; GSM49611     6  0.0000      0.892 0.000 0.000 0.000 0.0 0.000 1.000
#&gt; GSM49612     6  0.0000      0.892 0.000 0.000 0.000 0.0 0.000 1.000
#&gt; GSM49614     3  0.0000      0.994 0.000 0.000 1.000 0.0 0.000 0.000
#&gt; GSM49615     3  0.0146      0.996 0.004 0.000 0.996 0.0 0.000 0.000
#&gt; GSM49616     3  0.0146      0.996 0.004 0.000 0.996 0.0 0.000 0.000
#&gt; GSM49617     3  0.0000      0.994 0.000 0.000 1.000 0.0 0.000 0.000
#&gt; GSM49564     5  0.3868      0.302 0.492 0.000 0.000 0.0 0.508 0.000
#&gt; GSM49565     1  0.3930      0.374 0.576 0.000 0.004 0.0 0.420 0.000
#&gt; GSM49566     1  0.3782      0.463 0.636 0.000 0.004 0.0 0.360 0.000
#&gt; GSM49567     1  0.3907      0.398 0.588 0.000 0.004 0.0 0.408 0.000
#&gt; GSM49568     1  0.1765      0.774 0.904 0.000 0.000 0.0 0.096 0.000
#&gt; GSM49569     5  0.0865      0.742 0.036 0.000 0.000 0.0 0.964 0.000
#&gt; GSM49570     4  0.0000      1.000 0.000 0.000 0.000 1.0 0.000 0.000
#&gt; GSM49571     5  0.1387      0.744 0.068 0.000 0.000 0.0 0.932 0.000
#&gt; GSM49572     1  0.2838      0.682 0.808 0.000 0.004 0.0 0.188 0.000
#&gt; GSM49573     4  0.0000      1.000 0.000 0.000 0.000 1.0 0.000 0.000
#&gt; GSM49574     1  0.3737      0.295 0.608 0.000 0.000 0.0 0.392 0.000
#&gt; GSM49575     5  0.0146      0.733 0.000 0.000 0.004 0.0 0.996 0.000
#&gt; GSM49576     5  0.0405      0.734 0.008 0.000 0.004 0.0 0.988 0.000
#&gt; GSM49577     5  0.0146      0.733 0.000 0.000 0.004 0.0 0.996 0.000
#&gt; GSM49578     1  0.0000      0.812 1.000 0.000 0.000 0.0 0.000 0.000
#&gt; GSM49579     5  0.2668      0.607 0.168 0.000 0.004 0.0 0.828 0.000
#&gt; GSM49580     1  0.0547      0.807 0.980 0.000 0.000 0.0 0.020 0.000
#&gt; GSM49581     1  0.0458      0.807 0.984 0.000 0.000 0.0 0.016 0.000
#&gt; GSM49582     1  0.0000      0.812 1.000 0.000 0.000 0.0 0.000 0.000
#&gt; GSM49583     2  0.0000      0.983 0.000 1.000 0.000 0.0 0.000 0.000
#&gt; GSM49584     1  0.1387      0.783 0.932 0.000 0.000 0.0 0.068 0.000
#&gt; GSM49585     1  0.0000      0.812 1.000 0.000 0.000 0.0 0.000 0.000
#&gt; GSM49586     5  0.2562      0.718 0.172 0.000 0.000 0.0 0.828 0.000
#&gt; GSM49587     1  0.0000      0.812 1.000 0.000 0.000 0.0 0.000 0.000
#&gt; GSM49588     1  0.0000      0.812 1.000 0.000 0.000 0.0 0.000 0.000
#&gt; GSM49589     5  0.3797      0.448 0.420 0.000 0.000 0.0 0.580 0.000
#&gt; GSM49590     5  0.2703      0.691 0.172 0.000 0.004 0.0 0.824 0.000
#&gt; GSM49591     1  0.0000      0.812 1.000 0.000 0.000 0.0 0.000 0.000
#&gt; GSM49592     1  0.0000      0.812 1.000 0.000 0.000 0.0 0.000 0.000
#&gt; GSM49593     5  0.3578      0.596 0.340 0.000 0.000 0.0 0.660 0.000
#&gt; GSM49594     5  0.0146      0.733 0.000 0.000 0.004 0.0 0.996 0.000
#&gt; GSM49595     5  0.0291      0.735 0.004 0.000 0.004 0.0 0.992 0.000
#&gt; GSM49596     1  0.2562      0.662 0.828 0.000 0.000 0.0 0.172 0.000
#&gt; GSM49597     2  0.0000      0.983 0.000 1.000 0.000 0.0 0.000 0.000
#&gt; GSM49598     5  0.3446      0.628 0.308 0.000 0.000 0.0 0.692 0.000
#&gt; GSM49599     5  0.3860     -0.205 0.472 0.000 0.000 0.0 0.528 0.000
#&gt; GSM49600     1  0.0260      0.809 0.992 0.000 0.000 0.0 0.008 0.000
#&gt; GSM49601     1  0.3857     -0.274 0.532 0.000 0.000 0.0 0.468 0.000
#&gt; GSM49602     5  0.2454      0.723 0.160 0.000 0.000 0.0 0.840 0.000
#&gt; GSM49603     5  0.3531      0.607 0.328 0.000 0.000 0.0 0.672 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-5-a').click(function(){
  $('#tab-ATC-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-get-signatures'>
<ul>
<li><a href='#tab-ATC-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-1-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-1"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-2-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-2"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-3-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-3"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-4-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-4"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-5-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-pam-signature_compare](figure_cola/ATC-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-pam-collect-classes](figure_cola/ATC-pam-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>          n disease.state(p) cell.type(p) k
#> ATC:pam 54         5.97e-07     6.72e-04 2
#> ATC:pam 42         4.32e-06     3.71e-10 3
#> ATC:pam 51         8.01e-07     3.06e-12 4
#> ATC:pam 34         2.91e-04     7.90e-06 5
#> ATC:pam 46         3.94e-06     2.25e-11 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:mclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "mclust"]
# you can also extract it by
# res = res_list["ATC:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21168 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-mclust-collect-plots](figure_cola/ATC-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-mclust-select-partition-number](figure_cola/ATC-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.977       0.989         0.4786 0.516   0.516
#> 3 3 0.792           0.911       0.922         0.2233 0.893   0.796
#> 4 4 0.806           0.925       0.937         0.0454 0.976   0.942
#> 5 5 0.725           0.859       0.873         0.1410 0.878   0.694
#> 6 6 0.675           0.838       0.872         0.0759 0.992   0.970
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-get-classes'>
<ul>
<li><a href='#tab-ATC-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-mclust-get-classes-1'>
<p><a id='tab-ATC-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM49613     2  0.7219      0.775 0.200 0.800
#&gt; GSM49604     2  0.0000      0.970 0.000 1.000
#&gt; GSM49605     2  0.0000      0.970 0.000 1.000
#&gt; GSM49606     2  0.0000      0.970 0.000 1.000
#&gt; GSM49607     2  0.0000      0.970 0.000 1.000
#&gt; GSM49608     2  0.0000      0.970 0.000 1.000
#&gt; GSM49609     2  0.0000      0.970 0.000 1.000
#&gt; GSM49610     2  0.0000      0.970 0.000 1.000
#&gt; GSM49611     2  0.0000      0.970 0.000 1.000
#&gt; GSM49612     2  0.0000      0.970 0.000 1.000
#&gt; GSM49614     2  0.0000      0.970 0.000 1.000
#&gt; GSM49615     2  0.7219      0.775 0.200 0.800
#&gt; GSM49616     2  0.0376      0.967 0.004 0.996
#&gt; GSM49617     2  0.0000      0.970 0.000 1.000
#&gt; GSM49564     1  0.0672      0.992 0.992 0.008
#&gt; GSM49565     1  0.0000      1.000 1.000 0.000
#&gt; GSM49566     1  0.0000      1.000 1.000 0.000
#&gt; GSM49567     1  0.0000      1.000 1.000 0.000
#&gt; GSM49568     1  0.0000      1.000 1.000 0.000
#&gt; GSM49569     1  0.0000      1.000 1.000 0.000
#&gt; GSM49570     2  0.0000      0.970 0.000 1.000
#&gt; GSM49571     2  0.0000      0.970 0.000 1.000
#&gt; GSM49572     1  0.0000      1.000 1.000 0.000
#&gt; GSM49573     2  0.0000      0.970 0.000 1.000
#&gt; GSM49574     1  0.0000      1.000 1.000 0.000
#&gt; GSM49575     2  0.7056      0.785 0.192 0.808
#&gt; GSM49576     1  0.0000      1.000 1.000 0.000
#&gt; GSM49577     1  0.0000      1.000 1.000 0.000
#&gt; GSM49578     1  0.0000      1.000 1.000 0.000
#&gt; GSM49579     1  0.0000      1.000 1.000 0.000
#&gt; GSM49580     1  0.0000      1.000 1.000 0.000
#&gt; GSM49581     1  0.0000      1.000 1.000 0.000
#&gt; GSM49582     1  0.0000      1.000 1.000 0.000
#&gt; GSM49583     2  0.0000      0.970 0.000 1.000
#&gt; GSM49584     1  0.0000      1.000 1.000 0.000
#&gt; GSM49585     1  0.0000      1.000 1.000 0.000
#&gt; GSM49586     2  0.0000      0.970 0.000 1.000
#&gt; GSM49587     1  0.0000      1.000 1.000 0.000
#&gt; GSM49588     1  0.0000      1.000 1.000 0.000
#&gt; GSM49589     1  0.0000      1.000 1.000 0.000
#&gt; GSM49590     1  0.0000      1.000 1.000 0.000
#&gt; GSM49591     1  0.0000      1.000 1.000 0.000
#&gt; GSM49592     1  0.0000      1.000 1.000 0.000
#&gt; GSM49593     1  0.0000      1.000 1.000 0.000
#&gt; GSM49594     1  0.0000      1.000 1.000 0.000
#&gt; GSM49595     1  0.0000      1.000 1.000 0.000
#&gt; GSM49596     1  0.0000      1.000 1.000 0.000
#&gt; GSM49597     2  0.0000      0.970 0.000 1.000
#&gt; GSM49598     1  0.0000      1.000 1.000 0.000
#&gt; GSM49599     1  0.0000      1.000 1.000 0.000
#&gt; GSM49600     1  0.0000      1.000 1.000 0.000
#&gt; GSM49601     1  0.0000      1.000 1.000 0.000
#&gt; GSM49602     1  0.0000      1.000 1.000 0.000
#&gt; GSM49603     1  0.0000      1.000 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-1-a').click(function(){
  $('#tab-ATC-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-2'>
<p><a id='tab-ATC-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM49613     3  0.2878      0.834 0.000 0.096 0.904
#&gt; GSM49604     3  0.0000      0.892 0.000 0.000 1.000
#&gt; GSM49605     2  0.4555      1.000 0.000 0.800 0.200
#&gt; GSM49606     2  0.4555      1.000 0.000 0.800 0.200
#&gt; GSM49607     2  0.4555      1.000 0.000 0.800 0.200
#&gt; GSM49608     2  0.4555      1.000 0.000 0.800 0.200
#&gt; GSM49609     2  0.4555      1.000 0.000 0.800 0.200
#&gt; GSM49610     2  0.4555      1.000 0.000 0.800 0.200
#&gt; GSM49611     2  0.4555      1.000 0.000 0.800 0.200
#&gt; GSM49612     2  0.4555      1.000 0.000 0.800 0.200
#&gt; GSM49614     3  0.0000      0.892 0.000 0.000 1.000
#&gt; GSM49615     3  0.2878      0.834 0.000 0.096 0.904
#&gt; GSM49616     3  0.0000      0.892 0.000 0.000 1.000
#&gt; GSM49617     3  0.0000      0.892 0.000 0.000 1.000
#&gt; GSM49564     1  0.7592      0.641 0.680 0.112 0.208
#&gt; GSM49565     1  0.0000      0.947 1.000 0.000 0.000
#&gt; GSM49566     1  0.0000      0.947 1.000 0.000 0.000
#&gt; GSM49567     1  0.0000      0.947 1.000 0.000 0.000
#&gt; GSM49568     1  0.0000      0.947 1.000 0.000 0.000
#&gt; GSM49569     1  0.0000      0.947 1.000 0.000 0.000
#&gt; GSM49570     3  0.0000      0.892 0.000 0.000 1.000
#&gt; GSM49571     3  0.0000      0.892 0.000 0.000 1.000
#&gt; GSM49572     1  0.0000      0.947 1.000 0.000 0.000
#&gt; GSM49573     3  0.0000      0.892 0.000 0.000 1.000
#&gt; GSM49574     1  0.0000      0.947 1.000 0.000 0.000
#&gt; GSM49575     3  0.4121      0.724 0.168 0.000 0.832
#&gt; GSM49576     1  0.0000      0.947 1.000 0.000 0.000
#&gt; GSM49577     1  0.1411      0.925 0.964 0.000 0.036
#&gt; GSM49578     1  0.2625      0.911 0.916 0.084 0.000
#&gt; GSM49579     1  0.0000      0.947 1.000 0.000 0.000
#&gt; GSM49580     1  0.0000      0.947 1.000 0.000 0.000
#&gt; GSM49581     3  0.6264      0.430 0.380 0.004 0.616
#&gt; GSM49582     1  0.0237      0.946 0.996 0.004 0.000
#&gt; GSM49583     2  0.4555      1.000 0.000 0.800 0.200
#&gt; GSM49584     1  0.0000      0.947 1.000 0.000 0.000
#&gt; GSM49585     1  0.4555      0.841 0.800 0.200 0.000
#&gt; GSM49586     3  0.0000      0.892 0.000 0.000 1.000
#&gt; GSM49587     1  0.0000      0.947 1.000 0.000 0.000
#&gt; GSM49588     1  0.4555      0.841 0.800 0.200 0.000
#&gt; GSM49589     1  0.4555      0.841 0.800 0.200 0.000
#&gt; GSM49590     1  0.0000      0.947 1.000 0.000 0.000
#&gt; GSM49591     1  0.4121      0.863 0.832 0.168 0.000
#&gt; GSM49592     1  0.4555      0.841 0.800 0.200 0.000
#&gt; GSM49593     1  0.0237      0.946 0.996 0.004 0.000
#&gt; GSM49594     1  0.0000      0.947 1.000 0.000 0.000
#&gt; GSM49595     1  0.0000      0.947 1.000 0.000 0.000
#&gt; GSM49596     1  0.0000      0.947 1.000 0.000 0.000
#&gt; GSM49597     2  0.4555      1.000 0.000 0.800 0.200
#&gt; GSM49598     1  0.0237      0.946 0.996 0.004 0.000
#&gt; GSM49599     1  0.0000      0.947 1.000 0.000 0.000
#&gt; GSM49600     1  0.0000      0.947 1.000 0.000 0.000
#&gt; GSM49601     1  0.4555      0.841 0.800 0.200 0.000
#&gt; GSM49602     1  0.1529      0.932 0.960 0.040 0.000
#&gt; GSM49603     1  0.3619      0.883 0.864 0.136 0.000
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-2-a').click(function(){
  $('#tab-ATC-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-3'>
<p><a id='tab-ATC-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM49613     3  0.0188      1.000 0.000 0.004 0.996 0.000
#&gt; GSM49604     4  0.3569      0.855 0.000 0.196 0.000 0.804
#&gt; GSM49605     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM49606     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM49607     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM49608     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM49609     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM49610     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM49611     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM49612     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM49614     3  0.0188      1.000 0.000 0.004 0.996 0.000
#&gt; GSM49615     3  0.0188      1.000 0.000 0.004 0.996 0.000
#&gt; GSM49616     3  0.0188      1.000 0.000 0.004 0.996 0.000
#&gt; GSM49617     3  0.0188      1.000 0.000 0.004 0.996 0.000
#&gt; GSM49564     1  0.3257      0.867 0.844 0.000 0.004 0.152
#&gt; GSM49565     1  0.0188      0.945 0.996 0.000 0.000 0.004
#&gt; GSM49566     1  0.0188      0.945 0.996 0.000 0.000 0.004
#&gt; GSM49567     1  0.0188      0.945 0.996 0.000 0.000 0.004
#&gt; GSM49568     1  0.0000      0.945 1.000 0.000 0.000 0.000
#&gt; GSM49569     1  0.0188      0.945 0.996 0.000 0.000 0.004
#&gt; GSM49570     4  0.3569      0.855 0.000 0.196 0.000 0.804
#&gt; GSM49571     4  0.3710      0.854 0.000 0.192 0.004 0.804
#&gt; GSM49572     1  0.0188      0.945 0.996 0.000 0.000 0.004
#&gt; GSM49573     4  0.3569      0.855 0.000 0.196 0.000 0.804
#&gt; GSM49574     1  0.0188      0.945 0.996 0.000 0.000 0.004
#&gt; GSM49575     4  0.5459      0.828 0.072 0.192 0.004 0.732
#&gt; GSM49576     1  0.0000      0.945 1.000 0.000 0.000 0.000
#&gt; GSM49577     1  0.0895      0.933 0.976 0.020 0.004 0.000
#&gt; GSM49578     1  0.3052      0.877 0.860 0.000 0.004 0.136
#&gt; GSM49579     1  0.0188      0.945 0.996 0.000 0.000 0.004
#&gt; GSM49580     1  0.0000      0.945 1.000 0.000 0.000 0.000
#&gt; GSM49581     4  0.4585      0.470 0.332 0.000 0.000 0.668
#&gt; GSM49582     1  0.0000      0.945 1.000 0.000 0.000 0.000
#&gt; GSM49583     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM49584     1  0.0000      0.945 1.000 0.000 0.000 0.000
#&gt; GSM49585     1  0.3710      0.839 0.804 0.000 0.004 0.192
#&gt; GSM49586     4  0.5429      0.830 0.068 0.196 0.004 0.732
#&gt; GSM49587     1  0.0000      0.945 1.000 0.000 0.000 0.000
#&gt; GSM49588     1  0.3710      0.839 0.804 0.000 0.004 0.192
#&gt; GSM49589     1  0.3583      0.848 0.816 0.000 0.004 0.180
#&gt; GSM49590     1  0.0188      0.945 0.996 0.000 0.000 0.004
#&gt; GSM49591     1  0.3583      0.848 0.816 0.000 0.004 0.180
#&gt; GSM49592     1  0.3710      0.839 0.804 0.000 0.004 0.192
#&gt; GSM49593     1  0.0000      0.945 1.000 0.000 0.000 0.000
#&gt; GSM49594     1  0.0188      0.945 0.996 0.000 0.000 0.004
#&gt; GSM49595     1  0.0188      0.945 0.996 0.000 0.000 0.004
#&gt; GSM49596     1  0.0000      0.945 1.000 0.000 0.000 0.000
#&gt; GSM49597     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM49598     1  0.0000      0.945 1.000 0.000 0.000 0.000
#&gt; GSM49599     1  0.0188      0.945 0.996 0.000 0.000 0.004
#&gt; GSM49600     1  0.0000      0.945 1.000 0.000 0.000 0.000
#&gt; GSM49601     1  0.3710      0.839 0.804 0.000 0.004 0.192
#&gt; GSM49602     1  0.0469      0.942 0.988 0.000 0.000 0.012
#&gt; GSM49603     1  0.3355      0.862 0.836 0.000 0.004 0.160
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-3-a').click(function(){
  $('#tab-ATC-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-4'>
<p><a id='tab-ATC-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM49613     3  0.1082      0.954 0.028 0.000 0.964 0.000 0.008
#&gt; GSM49604     4  0.0000      0.791 0.000 0.000 0.000 1.000 0.000
#&gt; GSM49605     2  0.0000      0.988 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49606     2  0.0000      0.988 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49607     2  0.0162      0.988 0.000 0.996 0.000 0.004 0.000
#&gt; GSM49608     2  0.0000      0.988 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49609     2  0.0794      0.979 0.000 0.972 0.000 0.028 0.000
#&gt; GSM49610     2  0.0162      0.988 0.000 0.996 0.000 0.004 0.000
#&gt; GSM49611     2  0.0794      0.979 0.000 0.972 0.000 0.028 0.000
#&gt; GSM49612     2  0.0794      0.979 0.000 0.972 0.000 0.028 0.000
#&gt; GSM49614     3  0.0162      0.967 0.000 0.000 0.996 0.004 0.000
#&gt; GSM49615     3  0.1082      0.954 0.028 0.000 0.964 0.000 0.008
#&gt; GSM49616     3  0.0000      0.968 0.000 0.000 1.000 0.000 0.000
#&gt; GSM49617     3  0.0000      0.968 0.000 0.000 1.000 0.000 0.000
#&gt; GSM49564     5  0.4015      0.916 0.348 0.000 0.000 0.000 0.652
#&gt; GSM49565     1  0.1792      0.842 0.916 0.000 0.000 0.000 0.084
#&gt; GSM49566     5  0.4088      0.949 0.368 0.000 0.000 0.000 0.632
#&gt; GSM49567     1  0.2068      0.837 0.904 0.000 0.000 0.004 0.092
#&gt; GSM49568     1  0.1341      0.835 0.944 0.000 0.000 0.000 0.056
#&gt; GSM49569     5  0.4088      0.949 0.368 0.000 0.000 0.000 0.632
#&gt; GSM49570     4  0.0693      0.786 0.000 0.012 0.000 0.980 0.008
#&gt; GSM49571     4  0.0794      0.789 0.000 0.000 0.000 0.972 0.028
#&gt; GSM49572     1  0.1792      0.842 0.916 0.000 0.000 0.000 0.084
#&gt; GSM49573     4  0.0290      0.790 0.000 0.000 0.000 0.992 0.008
#&gt; GSM49574     1  0.1792      0.842 0.916 0.000 0.000 0.000 0.084
#&gt; GSM49575     4  0.5324      0.645 0.128 0.000 0.000 0.668 0.204
#&gt; GSM49576     5  0.4150      0.946 0.388 0.000 0.000 0.000 0.612
#&gt; GSM49577     1  0.2260      0.841 0.908 0.000 0.000 0.028 0.064
#&gt; GSM49578     1  0.1671      0.834 0.924 0.000 0.000 0.000 0.076
#&gt; GSM49579     1  0.2424      0.778 0.868 0.000 0.000 0.000 0.132
#&gt; GSM49580     1  0.1270      0.839 0.948 0.000 0.000 0.000 0.052
#&gt; GSM49581     4  0.6362      0.137 0.368 0.000 0.000 0.464 0.168
#&gt; GSM49582     1  0.0794      0.857 0.972 0.000 0.000 0.000 0.028
#&gt; GSM49583     2  0.0290      0.988 0.000 0.992 0.000 0.008 0.000
#&gt; GSM49584     5  0.4273      0.866 0.448 0.000 0.000 0.000 0.552
#&gt; GSM49585     1  0.2773      0.752 0.836 0.000 0.000 0.000 0.164
#&gt; GSM49586     4  0.3318      0.745 0.008 0.000 0.000 0.800 0.192
#&gt; GSM49587     1  0.0609      0.861 0.980 0.000 0.000 0.000 0.020
#&gt; GSM49588     1  0.2929      0.739 0.820 0.000 0.000 0.000 0.180
#&gt; GSM49589     5  0.4225      0.938 0.364 0.000 0.004 0.000 0.632
#&gt; GSM49590     5  0.4101      0.948 0.372 0.000 0.000 0.000 0.628
#&gt; GSM49591     1  0.2690      0.782 0.844 0.000 0.000 0.000 0.156
#&gt; GSM49592     1  0.2891      0.743 0.824 0.000 0.000 0.000 0.176
#&gt; GSM49593     1  0.0963      0.860 0.964 0.000 0.000 0.000 0.036
#&gt; GSM49594     1  0.1732      0.844 0.920 0.000 0.000 0.000 0.080
#&gt; GSM49595     1  0.1608      0.845 0.928 0.000 0.000 0.000 0.072
#&gt; GSM49596     1  0.0404      0.860 0.988 0.000 0.000 0.000 0.012
#&gt; GSM49597     2  0.0290      0.988 0.000 0.992 0.000 0.008 0.000
#&gt; GSM49598     1  0.1357      0.850 0.948 0.000 0.000 0.004 0.048
#&gt; GSM49599     1  0.1732      0.844 0.920 0.000 0.000 0.000 0.080
#&gt; GSM49600     1  0.1544      0.823 0.932 0.000 0.000 0.000 0.068
#&gt; GSM49601     1  0.2929      0.739 0.820 0.000 0.000 0.000 0.180
#&gt; GSM49602     1  0.0703      0.857 0.976 0.000 0.000 0.000 0.024
#&gt; GSM49603     1  0.2471      0.792 0.864 0.000 0.000 0.000 0.136
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-4-a').click(function(){
  $('#tab-ATC-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-5'>
<p><a id='tab-ATC-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM49613     3  0.1036      0.958 0.024 0.000 0.964 0.000 0.004 0.008
#&gt; GSM49604     4  0.0405      0.935 0.000 0.004 0.000 0.988 0.000 0.008
#&gt; GSM49605     2  0.0000      0.991 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49606     2  0.0000      0.991 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49607     2  0.0000      0.991 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49608     2  0.0000      0.991 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49609     2  0.0547      0.984 0.000 0.980 0.000 0.020 0.000 0.000
#&gt; GSM49610     2  0.0000      0.991 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM49611     2  0.0547      0.984 0.000 0.980 0.000 0.020 0.000 0.000
#&gt; GSM49612     2  0.0547      0.984 0.000 0.980 0.000 0.020 0.000 0.000
#&gt; GSM49614     3  0.0000      0.973 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49615     3  0.1036      0.958 0.024 0.000 0.964 0.000 0.004 0.008
#&gt; GSM49616     3  0.0000      0.973 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49617     3  0.0000      0.973 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49564     5  0.2278      0.877 0.128 0.000 0.000 0.004 0.868 0.000
#&gt; GSM49565     1  0.5333      0.721 0.564 0.000 0.000 0.000 0.300 0.136
#&gt; GSM49566     5  0.1007      0.942 0.044 0.000 0.000 0.000 0.956 0.000
#&gt; GSM49567     1  0.4765      0.764 0.676 0.000 0.000 0.000 0.152 0.172
#&gt; GSM49568     1  0.3221      0.776 0.736 0.000 0.000 0.000 0.264 0.000
#&gt; GSM49569     5  0.1265      0.942 0.044 0.000 0.008 0.000 0.948 0.000
#&gt; GSM49570     4  0.0363      0.933 0.000 0.012 0.000 0.988 0.000 0.000
#&gt; GSM49571     4  0.2260      0.825 0.000 0.000 0.000 0.860 0.000 0.140
#&gt; GSM49572     1  0.5319      0.722 0.568 0.000 0.000 0.000 0.296 0.136
#&gt; GSM49573     4  0.0146      0.937 0.000 0.004 0.000 0.996 0.000 0.000
#&gt; GSM49574     1  0.5350      0.721 0.564 0.000 0.000 0.000 0.296 0.140
#&gt; GSM49575     6  0.2979      0.585 0.032 0.000 0.000 0.112 0.008 0.848
#&gt; GSM49576     5  0.1141      0.943 0.052 0.000 0.000 0.000 0.948 0.000
#&gt; GSM49577     1  0.4707      0.756 0.676 0.000 0.000 0.000 0.120 0.204
#&gt; GSM49578     1  0.1010      0.778 0.960 0.000 0.000 0.000 0.036 0.004
#&gt; GSM49579     1  0.4273      0.680 0.596 0.000 0.000 0.000 0.380 0.024
#&gt; GSM49580     1  0.3221      0.776 0.736 0.000 0.000 0.000 0.264 0.000
#&gt; GSM49581     6  0.4117      0.489 0.256 0.000 0.000 0.004 0.036 0.704
#&gt; GSM49582     1  0.2948      0.781 0.848 0.000 0.000 0.000 0.060 0.092
#&gt; GSM49583     2  0.0405      0.986 0.000 0.988 0.000 0.004 0.000 0.008
#&gt; GSM49584     5  0.1387      0.937 0.068 0.000 0.000 0.000 0.932 0.000
#&gt; GSM49585     1  0.0603      0.753 0.980 0.000 0.000 0.000 0.004 0.016
#&gt; GSM49586     6  0.3428      0.340 0.000 0.000 0.000 0.304 0.000 0.696
#&gt; GSM49587     1  0.2854      0.802 0.792 0.000 0.000 0.000 0.208 0.000
#&gt; GSM49588     1  0.0520      0.752 0.984 0.000 0.000 0.000 0.008 0.008
#&gt; GSM49589     5  0.2540      0.884 0.104 0.000 0.020 0.000 0.872 0.004
#&gt; GSM49590     5  0.0937      0.941 0.040 0.000 0.000 0.000 0.960 0.000
#&gt; GSM49591     1  0.2633      0.756 0.864 0.000 0.000 0.000 0.104 0.032
#&gt; GSM49592     1  0.0508      0.750 0.984 0.000 0.000 0.000 0.004 0.012
#&gt; GSM49593     1  0.3013      0.784 0.844 0.000 0.000 0.000 0.068 0.088
#&gt; GSM49594     1  0.4774      0.769 0.672 0.000 0.000 0.000 0.192 0.136
#&gt; GSM49595     1  0.4809      0.768 0.668 0.000 0.000 0.000 0.192 0.140
#&gt; GSM49596     1  0.2948      0.809 0.804 0.000 0.000 0.000 0.188 0.008
#&gt; GSM49597     2  0.0405      0.986 0.000 0.988 0.000 0.004 0.000 0.008
#&gt; GSM49598     1  0.3172      0.767 0.824 0.000 0.000 0.000 0.048 0.128
#&gt; GSM49599     1  0.4781      0.770 0.672 0.000 0.000 0.000 0.188 0.140
#&gt; GSM49600     1  0.3244      0.774 0.732 0.000 0.000 0.000 0.268 0.000
#&gt; GSM49601     1  0.0508      0.750 0.984 0.000 0.000 0.000 0.004 0.012
#&gt; GSM49602     1  0.3076      0.780 0.760 0.000 0.000 0.000 0.240 0.000
#&gt; GSM49603     1  0.2006      0.754 0.892 0.000 0.000 0.000 0.104 0.004
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-5-a').click(function(){
  $('#tab-ATC-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-get-signatures'>
<ul>
<li><a href='#tab-ATC-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-1-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-1"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-2-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-2"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-3-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-3"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-4-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-4"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-5-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-mclust-signature_compare](figure_cola/ATC-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-mclust-collect-classes](figure_cola/ATC-mclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n disease.state(p) cell.type(p) k
#> ATC:mclust 54         5.52e-05     2.06e-04 2
#> ATC:mclust 53         1.21e-07     6.60e-08 3
#> ATC:mclust 53         5.55e-07     2.20e-13 4
#> ATC:mclust 53         2.03e-06     2.70e-12 5
#> ATC:mclust 52         6.75e-06     4.64e-11 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:NMF**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "NMF"]
# you can also extract it by
# res = res_list["ATC:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21168 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-NMF-collect-plots](figure_cola/ATC-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-NMF-select-partition-number](figure_cola/ATC-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.3731 0.628   0.628
#> 3 3 0.968           0.961       0.976         0.2600 0.893   0.831
#> 4 4 0.710           0.839       0.903         0.2901 0.823   0.681
#> 5 5 0.668           0.736       0.865         0.0548 0.994   0.984
#> 6 6 0.568           0.662       0.790         0.0862 0.956   0.891
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-get-classes'>
<ul>
<li><a href='#tab-ATC-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-NMF-get-classes-1'>
<p><a id='tab-ATC-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette p1 p2
#&gt; GSM49613     1       0          1  1  0
#&gt; GSM49604     2       0          1  0  1
#&gt; GSM49605     2       0          1  0  1
#&gt; GSM49606     2       0          1  0  1
#&gt; GSM49607     2       0          1  0  1
#&gt; GSM49608     2       0          1  0  1
#&gt; GSM49609     2       0          1  0  1
#&gt; GSM49610     2       0          1  0  1
#&gt; GSM49611     2       0          1  0  1
#&gt; GSM49612     2       0          1  0  1
#&gt; GSM49614     1       0          1  1  0
#&gt; GSM49615     1       0          1  1  0
#&gt; GSM49616     1       0          1  1  0
#&gt; GSM49617     1       0          1  1  0
#&gt; GSM49564     1       0          1  1  0
#&gt; GSM49565     1       0          1  1  0
#&gt; GSM49566     1       0          1  1  0
#&gt; GSM49567     1       0          1  1  0
#&gt; GSM49568     1       0          1  1  0
#&gt; GSM49569     1       0          1  1  0
#&gt; GSM49570     2       0          1  0  1
#&gt; GSM49571     1       0          1  1  0
#&gt; GSM49572     1       0          1  1  0
#&gt; GSM49573     2       0          1  0  1
#&gt; GSM49574     1       0          1  1  0
#&gt; GSM49575     1       0          1  1  0
#&gt; GSM49576     1       0          1  1  0
#&gt; GSM49577     1       0          1  1  0
#&gt; GSM49578     1       0          1  1  0
#&gt; GSM49579     1       0          1  1  0
#&gt; GSM49580     1       0          1  1  0
#&gt; GSM49581     1       0          1  1  0
#&gt; GSM49582     1       0          1  1  0
#&gt; GSM49583     2       0          1  0  1
#&gt; GSM49584     1       0          1  1  0
#&gt; GSM49585     1       0          1  1  0
#&gt; GSM49586     1       0          1  1  0
#&gt; GSM49587     1       0          1  1  0
#&gt; GSM49588     1       0          1  1  0
#&gt; GSM49589     1       0          1  1  0
#&gt; GSM49590     1       0          1  1  0
#&gt; GSM49591     1       0          1  1  0
#&gt; GSM49592     1       0          1  1  0
#&gt; GSM49593     1       0          1  1  0
#&gt; GSM49594     1       0          1  1  0
#&gt; GSM49595     1       0          1  1  0
#&gt; GSM49596     1       0          1  1  0
#&gt; GSM49597     2       0          1  0  1
#&gt; GSM49598     1       0          1  1  0
#&gt; GSM49599     1       0          1  1  0
#&gt; GSM49600     1       0          1  1  0
#&gt; GSM49601     1       0          1  1  0
#&gt; GSM49602     1       0          1  1  0
#&gt; GSM49603     1       0          1  1  0
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-1-a').click(function(){
  $('#tab-ATC-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-2'>
<p><a id='tab-ATC-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM49613     1  0.1289      0.971 0.968 0.000 0.032
#&gt; GSM49604     3  0.3267      0.822 0.000 0.116 0.884
#&gt; GSM49605     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM49606     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM49607     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM49608     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM49609     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM49610     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM49611     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM49612     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM49614     1  0.1411      0.968 0.964 0.000 0.036
#&gt; GSM49615     1  0.1289      0.971 0.968 0.000 0.032
#&gt; GSM49616     1  0.1289      0.971 0.968 0.000 0.032
#&gt; GSM49617     1  0.1289      0.971 0.968 0.000 0.032
#&gt; GSM49564     1  0.1289      0.971 0.968 0.000 0.032
#&gt; GSM49565     1  0.0237      0.985 0.996 0.000 0.004
#&gt; GSM49566     1  0.0000      0.985 1.000 0.000 0.000
#&gt; GSM49567     1  0.0747      0.981 0.984 0.000 0.016
#&gt; GSM49568     1  0.0424      0.985 0.992 0.000 0.008
#&gt; GSM49569     1  0.0000      0.985 1.000 0.000 0.000
#&gt; GSM49570     3  0.4931      0.716 0.000 0.232 0.768
#&gt; GSM49571     3  0.0000      0.826 0.000 0.000 1.000
#&gt; GSM49572     1  0.0000      0.985 1.000 0.000 0.000
#&gt; GSM49573     3  0.2711      0.834 0.000 0.088 0.912
#&gt; GSM49574     1  0.0000      0.985 1.000 0.000 0.000
#&gt; GSM49575     3  0.4235      0.713 0.176 0.000 0.824
#&gt; GSM49576     1  0.0237      0.985 0.996 0.000 0.004
#&gt; GSM49577     1  0.0000      0.985 1.000 0.000 0.000
#&gt; GSM49578     1  0.0424      0.984 0.992 0.000 0.008
#&gt; GSM49579     1  0.0000      0.985 1.000 0.000 0.000
#&gt; GSM49580     1  0.0237      0.985 0.996 0.000 0.004
#&gt; GSM49581     1  0.3192      0.890 0.888 0.000 0.112
#&gt; GSM49582     1  0.1031      0.979 0.976 0.000 0.024
#&gt; GSM49583     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM49584     1  0.0237      0.985 0.996 0.000 0.004
#&gt; GSM49585     1  0.0592      0.984 0.988 0.000 0.012
#&gt; GSM49586     3  0.2711      0.805 0.088 0.000 0.912
#&gt; GSM49587     1  0.0000      0.985 1.000 0.000 0.000
#&gt; GSM49588     1  0.1031      0.979 0.976 0.000 0.024
#&gt; GSM49589     1  0.0892      0.978 0.980 0.000 0.020
#&gt; GSM49590     1  0.0424      0.984 0.992 0.000 0.008
#&gt; GSM49591     1  0.0892      0.979 0.980 0.000 0.020
#&gt; GSM49592     1  0.0892      0.981 0.980 0.000 0.020
#&gt; GSM49593     1  0.0592      0.983 0.988 0.000 0.012
#&gt; GSM49594     1  0.0000      0.985 1.000 0.000 0.000
#&gt; GSM49595     1  0.0000      0.985 1.000 0.000 0.000
#&gt; GSM49596     1  0.0000      0.985 1.000 0.000 0.000
#&gt; GSM49597     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM49598     1  0.1753      0.961 0.952 0.000 0.048
#&gt; GSM49599     1  0.0237      0.985 0.996 0.000 0.004
#&gt; GSM49600     1  0.0237      0.985 0.996 0.000 0.004
#&gt; GSM49601     1  0.0747      0.981 0.984 0.000 0.016
#&gt; GSM49602     1  0.0237      0.985 0.996 0.000 0.004
#&gt; GSM49603     1  0.1163      0.974 0.972 0.000 0.028
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-2-a').click(function(){
  $('#tab-ATC-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-3'>
<p><a id='tab-ATC-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM49613     3  0.3610      0.927 0.200 0.000 0.800 0.000
#&gt; GSM49604     4  0.0817      0.869 0.000 0.024 0.000 0.976
#&gt; GSM49605     2  0.0000      0.979 0.000 1.000 0.000 0.000
#&gt; GSM49606     2  0.0592      0.977 0.000 0.984 0.016 0.000
#&gt; GSM49607     2  0.0817      0.974 0.000 0.976 0.024 0.000
#&gt; GSM49608     2  0.0000      0.979 0.000 1.000 0.000 0.000
#&gt; GSM49609     2  0.1732      0.961 0.004 0.948 0.040 0.008
#&gt; GSM49610     2  0.0336      0.978 0.000 0.992 0.008 0.000
#&gt; GSM49611     2  0.1109      0.972 0.000 0.968 0.028 0.004
#&gt; GSM49612     2  0.1109      0.972 0.000 0.968 0.028 0.004
#&gt; GSM49614     3  0.3950      0.913 0.184 0.008 0.804 0.004
#&gt; GSM49615     3  0.3610      0.927 0.200 0.000 0.800 0.000
#&gt; GSM49616     3  0.3528      0.925 0.192 0.000 0.808 0.000
#&gt; GSM49617     3  0.3668      0.921 0.188 0.004 0.808 0.000
#&gt; GSM49564     3  0.4406      0.843 0.300 0.000 0.700 0.000
#&gt; GSM49565     1  0.1118      0.880 0.964 0.000 0.036 0.000
#&gt; GSM49566     1  0.3074      0.764 0.848 0.000 0.152 0.000
#&gt; GSM49567     1  0.1118      0.880 0.964 0.000 0.036 0.000
#&gt; GSM49568     1  0.1389      0.884 0.952 0.000 0.048 0.000
#&gt; GSM49569     1  0.4730      0.248 0.636 0.000 0.364 0.000
#&gt; GSM49570     4  0.4888      0.786 0.000 0.124 0.096 0.780
#&gt; GSM49571     4  0.0188      0.868 0.000 0.000 0.004 0.996
#&gt; GSM49572     1  0.0469      0.890 0.988 0.000 0.012 0.000
#&gt; GSM49573     4  0.2596      0.864 0.000 0.024 0.068 0.908
#&gt; GSM49574     1  0.0592      0.888 0.984 0.000 0.016 0.000
#&gt; GSM49575     1  0.6698      0.245 0.556 0.000 0.104 0.340
#&gt; GSM49576     1  0.4331      0.503 0.712 0.000 0.288 0.000
#&gt; GSM49577     1  0.1474      0.870 0.948 0.000 0.052 0.000
#&gt; GSM49578     1  0.0592      0.888 0.984 0.000 0.016 0.000
#&gt; GSM49579     1  0.1389      0.875 0.952 0.000 0.048 0.000
#&gt; GSM49580     1  0.1637      0.880 0.940 0.000 0.060 0.000
#&gt; GSM49581     1  0.2494      0.855 0.916 0.000 0.036 0.048
#&gt; GSM49582     1  0.0376      0.891 0.992 0.000 0.004 0.004
#&gt; GSM49583     2  0.0707      0.975 0.000 0.980 0.020 0.000
#&gt; GSM49584     1  0.2760      0.795 0.872 0.000 0.128 0.000
#&gt; GSM49585     1  0.2255      0.861 0.920 0.000 0.068 0.012
#&gt; GSM49586     4  0.3946      0.675 0.168 0.000 0.020 0.812
#&gt; GSM49587     1  0.1022      0.885 0.968 0.000 0.032 0.000
#&gt; GSM49588     1  0.1398      0.881 0.956 0.000 0.040 0.004
#&gt; GSM49589     3  0.4454      0.828 0.308 0.000 0.692 0.000
#&gt; GSM49590     1  0.4989     -0.246 0.528 0.000 0.472 0.000
#&gt; GSM49591     1  0.1297      0.886 0.964 0.000 0.020 0.016
#&gt; GSM49592     1  0.2101      0.873 0.928 0.000 0.060 0.012
#&gt; GSM49593     1  0.0336      0.889 0.992 0.000 0.008 0.000
#&gt; GSM49594     1  0.0921      0.884 0.972 0.000 0.028 0.000
#&gt; GSM49595     1  0.0469      0.889 0.988 0.000 0.012 0.000
#&gt; GSM49596     1  0.0336      0.889 0.992 0.000 0.008 0.000
#&gt; GSM49597     2  0.1211      0.965 0.000 0.960 0.040 0.000
#&gt; GSM49598     1  0.1520      0.883 0.956 0.000 0.020 0.024
#&gt; GSM49599     1  0.0817      0.885 0.976 0.000 0.024 0.000
#&gt; GSM49600     1  0.1792      0.863 0.932 0.000 0.068 0.000
#&gt; GSM49601     1  0.2256      0.871 0.924 0.000 0.056 0.020
#&gt; GSM49602     1  0.0469      0.890 0.988 0.000 0.012 0.000
#&gt; GSM49603     1  0.1297      0.888 0.964 0.000 0.020 0.016
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-3-a').click(function(){
  $('#tab-ATC-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-4'>
<p><a id='tab-ATC-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM49613     3  0.1662     0.9036 0.056 0.000 0.936 0.004 0.004
#&gt; GSM49604     4  0.2654     0.7979 0.000 0.032 0.000 0.884 0.084
#&gt; GSM49605     2  0.0000     0.6519 0.000 1.000 0.000 0.000 0.000
#&gt; GSM49606     2  0.2280     0.4958 0.000 0.880 0.000 0.000 0.120
#&gt; GSM49607     2  0.1478     0.6408 0.000 0.936 0.000 0.000 0.064
#&gt; GSM49608     2  0.0404     0.6460 0.000 0.988 0.000 0.000 0.012
#&gt; GSM49609     5  0.5608     0.0000 0.012 0.472 0.004 0.036 0.476
#&gt; GSM49610     2  0.0703     0.6516 0.000 0.976 0.000 0.000 0.024
#&gt; GSM49611     2  0.4276    -0.5578 0.000 0.616 0.000 0.004 0.380
#&gt; GSM49612     2  0.4973    -0.7019 0.000 0.564 0.024 0.004 0.408
#&gt; GSM49614     3  0.1282     0.8979 0.044 0.000 0.952 0.004 0.000
#&gt; GSM49615     3  0.1662     0.9036 0.056 0.000 0.936 0.004 0.004
#&gt; GSM49616     3  0.1282     0.8998 0.044 0.000 0.952 0.000 0.004
#&gt; GSM49617     3  0.1121     0.8999 0.044 0.000 0.956 0.000 0.000
#&gt; GSM49564     3  0.3210     0.7347 0.212 0.000 0.788 0.000 0.000
#&gt; GSM49565     1  0.1012     0.8971 0.968 0.000 0.020 0.000 0.012
#&gt; GSM49566     1  0.3242     0.8205 0.816 0.000 0.172 0.000 0.012
#&gt; GSM49567     1  0.1914     0.8875 0.932 0.000 0.032 0.004 0.032
#&gt; GSM49568     1  0.1818     0.9046 0.932 0.000 0.044 0.000 0.024
#&gt; GSM49569     1  0.3807     0.7387 0.748 0.000 0.240 0.000 0.012
#&gt; GSM49570     4  0.4486     0.7622 0.000 0.080 0.000 0.748 0.172
#&gt; GSM49571     4  0.0324     0.8202 0.004 0.000 0.000 0.992 0.004
#&gt; GSM49572     1  0.1211     0.8975 0.960 0.000 0.016 0.000 0.024
#&gt; GSM49573     4  0.3236     0.8016 0.000 0.020 0.000 0.828 0.152
#&gt; GSM49574     1  0.0404     0.9009 0.988 0.000 0.012 0.000 0.000
#&gt; GSM49575     1  0.5444     0.6607 0.712 0.000 0.032 0.144 0.112
#&gt; GSM49576     1  0.3561     0.7132 0.740 0.000 0.260 0.000 0.000
#&gt; GSM49577     1  0.2227     0.8805 0.916 0.004 0.032 0.000 0.048
#&gt; GSM49578     1  0.1518     0.9007 0.944 0.000 0.048 0.004 0.004
#&gt; GSM49579     1  0.1597     0.9025 0.940 0.000 0.048 0.000 0.012
#&gt; GSM49580     1  0.1741     0.9042 0.936 0.000 0.040 0.000 0.024
#&gt; GSM49581     1  0.2273     0.8906 0.920 0.000 0.032 0.024 0.024
#&gt; GSM49582     1  0.0671     0.9054 0.980 0.000 0.016 0.000 0.004
#&gt; GSM49583     2  0.1792     0.6286 0.000 0.916 0.000 0.000 0.084
#&gt; GSM49584     1  0.2723     0.8639 0.864 0.000 0.124 0.000 0.012
#&gt; GSM49585     1  0.3047     0.8660 0.868 0.000 0.096 0.012 0.024
#&gt; GSM49586     4  0.4802     0.6648 0.068 0.000 0.004 0.716 0.212
#&gt; GSM49587     1  0.1608     0.8967 0.928 0.000 0.072 0.000 0.000
#&gt; GSM49588     1  0.1991     0.8903 0.916 0.000 0.076 0.004 0.004
#&gt; GSM49589     3  0.2773     0.8044 0.164 0.000 0.836 0.000 0.000
#&gt; GSM49590     1  0.4307     0.0401 0.504 0.000 0.496 0.000 0.000
#&gt; GSM49591     1  0.1314     0.8996 0.960 0.000 0.012 0.012 0.016
#&gt; GSM49592     1  0.2241     0.8877 0.908 0.000 0.076 0.008 0.008
#&gt; GSM49593     1  0.0798     0.9019 0.976 0.000 0.016 0.000 0.008
#&gt; GSM49594     1  0.0798     0.8997 0.976 0.000 0.016 0.000 0.008
#&gt; GSM49595     1  0.0566     0.9043 0.984 0.000 0.012 0.000 0.004
#&gt; GSM49596     1  0.0898     0.9049 0.972 0.000 0.020 0.000 0.008
#&gt; GSM49597     2  0.3707     0.3981 0.000 0.716 0.000 0.000 0.284
#&gt; GSM49598     1  0.0740     0.9034 0.980 0.000 0.008 0.004 0.008
#&gt; GSM49599     1  0.2278     0.8829 0.908 0.000 0.032 0.000 0.060
#&gt; GSM49600     1  0.2344     0.8961 0.904 0.000 0.064 0.000 0.032
#&gt; GSM49601     1  0.4711     0.7772 0.772 0.000 0.052 0.044 0.132
#&gt; GSM49602     1  0.1569     0.9027 0.944 0.000 0.044 0.004 0.008
#&gt; GSM49603     1  0.2199     0.8964 0.916 0.000 0.060 0.008 0.016
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-4-a').click(function(){
  $('#tab-ATC-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-5'>
<p><a id='tab-ATC-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM49613     3  0.1693      0.775 0.044 0.000 0.932 0.000 0.020 0.004
#&gt; GSM49604     4  0.5329      0.330 0.012 0.076 0.000 0.576 0.332 0.004
#&gt; GSM49605     2  0.3819      0.473 0.000 0.652 0.000 0.000 0.008 0.340
#&gt; GSM49606     2  0.3388      0.575 0.000 0.804 0.004 0.000 0.036 0.156
#&gt; GSM49607     2  0.3911      0.410 0.000 0.624 0.000 0.000 0.008 0.368
#&gt; GSM49608     2  0.3816      0.547 0.000 0.728 0.000 0.000 0.032 0.240
#&gt; GSM49609     2  0.3199      0.438 0.004 0.840 0.008 0.004 0.120 0.024
#&gt; GSM49610     2  0.3547      0.490 0.000 0.668 0.000 0.000 0.000 0.332
#&gt; GSM49611     2  0.1340      0.547 0.000 0.948 0.000 0.004 0.040 0.008
#&gt; GSM49612     2  0.2340      0.511 0.000 0.900 0.016 0.000 0.060 0.024
#&gt; GSM49614     3  0.1964      0.731 0.012 0.008 0.920 0.004 0.056 0.000
#&gt; GSM49615     3  0.1265      0.778 0.044 0.000 0.948 0.000 0.008 0.000
#&gt; GSM49616     3  0.1092      0.764 0.020 0.000 0.960 0.000 0.020 0.000
#&gt; GSM49617     3  0.1232      0.756 0.016 0.004 0.956 0.000 0.024 0.000
#&gt; GSM49564     3  0.3273      0.616 0.212 0.000 0.776 0.000 0.008 0.004
#&gt; GSM49565     1  0.3522      0.764 0.784 0.000 0.000 0.000 0.172 0.044
#&gt; GSM49566     1  0.4941      0.734 0.720 0.000 0.116 0.000 0.112 0.052
#&gt; GSM49567     1  0.2605      0.808 0.864 0.000 0.000 0.000 0.108 0.028
#&gt; GSM49568     1  0.2136      0.824 0.908 0.000 0.016 0.000 0.064 0.012
#&gt; GSM49569     1  0.4306      0.665 0.700 0.000 0.248 0.000 0.044 0.008
#&gt; GSM49570     4  0.1542      0.621 0.000 0.008 0.000 0.936 0.004 0.052
#&gt; GSM49571     4  0.3929      0.496 0.000 0.028 0.000 0.700 0.272 0.000
#&gt; GSM49572     1  0.3860      0.755 0.764 0.000 0.000 0.000 0.164 0.072
#&gt; GSM49573     4  0.0405      0.641 0.000 0.004 0.000 0.988 0.000 0.008
#&gt; GSM49574     1  0.0922      0.823 0.968 0.000 0.004 0.000 0.024 0.004
#&gt; GSM49575     1  0.6062      0.246 0.536 0.000 0.004 0.320 0.092 0.048
#&gt; GSM49576     1  0.4566      0.440 0.596 0.000 0.364 0.000 0.036 0.004
#&gt; GSM49577     1  0.4902      0.689 0.708 0.012 0.008 0.000 0.144 0.128
#&gt; GSM49578     1  0.2063      0.822 0.912 0.000 0.020 0.000 0.060 0.008
#&gt; GSM49579     1  0.2771      0.811 0.852 0.000 0.032 0.000 0.116 0.000
#&gt; GSM49580     1  0.1873      0.827 0.924 0.000 0.008 0.000 0.048 0.020
#&gt; GSM49581     1  0.2901      0.808 0.868 0.000 0.004 0.012 0.080 0.036
#&gt; GSM49582     1  0.0891      0.826 0.968 0.000 0.008 0.000 0.024 0.000
#&gt; GSM49583     6  0.4506      0.470 0.000 0.348 0.000 0.000 0.044 0.608
#&gt; GSM49584     1  0.4576      0.763 0.752 0.000 0.092 0.000 0.108 0.048
#&gt; GSM49585     1  0.3759      0.779 0.792 0.012 0.024 0.000 0.160 0.012
#&gt; GSM49586     5  0.7081      0.000 0.132 0.128 0.008 0.252 0.480 0.000
#&gt; GSM49587     1  0.3123      0.808 0.852 0.000 0.024 0.000 0.088 0.036
#&gt; GSM49588     1  0.2249      0.824 0.900 0.000 0.032 0.000 0.064 0.004
#&gt; GSM49589     3  0.2889      0.730 0.116 0.000 0.852 0.000 0.020 0.012
#&gt; GSM49590     3  0.4777      0.157 0.416 0.000 0.540 0.000 0.036 0.008
#&gt; GSM49591     1  0.1644      0.822 0.920 0.000 0.004 0.000 0.076 0.000
#&gt; GSM49592     1  0.2958      0.815 0.852 0.000 0.028 0.000 0.108 0.012
#&gt; GSM49593     1  0.3031      0.808 0.852 0.000 0.016 0.000 0.100 0.032
#&gt; GSM49594     1  0.3629      0.783 0.816 0.000 0.012 0.004 0.108 0.060
#&gt; GSM49595     1  0.2554      0.810 0.880 0.000 0.020 0.000 0.088 0.012
#&gt; GSM49596     1  0.0951      0.826 0.968 0.000 0.004 0.000 0.020 0.008
#&gt; GSM49597     6  0.2773      0.607 0.000 0.164 0.000 0.004 0.004 0.828
#&gt; GSM49598     1  0.4005      0.775 0.796 0.000 0.028 0.012 0.128 0.036
#&gt; GSM49599     1  0.4583      0.715 0.716 0.008 0.008 0.000 0.068 0.200
#&gt; GSM49600     1  0.3409      0.814 0.840 0.000 0.060 0.000 0.064 0.036
#&gt; GSM49601     1  0.5232      0.568 0.636 0.052 0.036 0.000 0.272 0.004
#&gt; GSM49602     1  0.3718      0.774 0.804 0.000 0.052 0.000 0.124 0.020
#&gt; GSM49603     1  0.4128      0.753 0.768 0.000 0.064 0.000 0.148 0.020
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-5-a').click(function(){
  $('#tab-ATC-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-get-signatures'>
<ul>
<li><a href='#tab-ATC-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-1-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-1"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-2-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-2"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-3-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-3"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-4-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-4"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-5-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-NMF-signature_compare](figure_cola/ATC-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-NMF-collect-classes](figure_cola/ATC-NMF-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>          n disease.state(p) cell.type(p) k
#> ATC:NMF 54         5.97e-07     6.72e-04 2
#> ATC:NMF 54         1.50e-07     3.78e-04 3
#> ATC:NMF 51         1.17e-06     1.72e-09 4
#> ATC:NMF 48         1.19e-05     2.48e-07 5
#> ATC:NMF 43         5.46e-07     5.54e-08 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

## Session info


```r
sessionInfo()
```

```
#> R version 3.6.0 (2019-04-26)
#> Platform: x86_64-pc-linux-gnu (64-bit)
#> Running under: CentOS Linux 7 (Core)
#> 
#> Matrix products: default
#> BLAS:   /usr/lib64/libblas.so.3.4.2
#> LAPACK: /usr/lib64/liblapack.so.3.4.2
#> 
#> locale:
#>  [1] LC_CTYPE=en_GB.UTF-8       LC_NUMERIC=C               LC_TIME=en_GB.UTF-8       
#>  [4] LC_COLLATE=en_GB.UTF-8     LC_MONETARY=en_GB.UTF-8    LC_MESSAGES=en_GB.UTF-8   
#>  [7] LC_PAPER=en_GB.UTF-8       LC_NAME=C                  LC_ADDRESS=C              
#> [10] LC_TELEPHONE=C             LC_MEASUREMENT=en_GB.UTF-8 LC_IDENTIFICATION=C       
#> 
#> attached base packages:
#> [1] grid      parallel  stats     graphics  grDevices utils     datasets  methods   base     
#> 
#> other attached packages:
#>  [1] genefilter_1.66.0     ComplexHeatmap_2.1.1  markdown_1.1          knitr_1.26           
#>  [5] preprocessCore_1.46.0 cola_1.3.2            GEOquery_2.52.0       Biobase_2.44.0       
#>  [9] BiocGenerics_0.30.0   GetoptLong_0.1.7     
#> 
#> loaded via a namespace (and not attached):
#>  [1] bitops_1.0-6         matrixStats_0.55.0   bit64_0.9-7          doParallel_1.0.15   
#>  [5] RColorBrewer_1.1-2   httr_1.4.1           tools_3.6.0          backports_1.1.5     
#>  [9] R6_2.4.1             DBI_1.0.0            lazyeval_0.2.2       colorspace_1.4-1    
#> [13] withr_2.1.2          tidyselect_0.2.5     gridExtra_2.3        bit_1.1-14          
#> [17] compiler_3.6.0       xml2_1.2.2           microbenchmark_1.4-7 pkgmaker_0.28       
#> [21] slam_0.1-46          scales_1.1.0         readr_1.3.1          NMF_0.23.6          
#> [25] stringr_1.4.0        digest_0.6.23        pkgconfig_2.0.3      bibtex_0.4.2        
#> [29] highr_0.8            limma_3.40.6         rlang_0.4.2          GlobalOptions_0.1.1 
#> [33] RSQLite_2.1.2        impute_1.58.0        shape_1.4.4          mclust_5.4.5        
#> [37] dendextend_1.12.0    dplyr_0.8.3          RCurl_1.95-4.12      magrittr_1.5        
#> [41] Matrix_1.2-17        Rcpp_1.0.3           munsell_0.5.0        S4Vectors_0.22.1    
#> [45] viridis_0.5.1        lifecycle_0.1.0      stringi_1.4.3        plyr_1.8.4          
#> [49] blob_1.2.0           crayon_1.3.4         lattice_0.20-38      splines_3.6.0       
#> [53] annotate_1.62.0      circlize_0.4.9       hms_0.5.2            zeallot_0.1.0       
#> [57] pillar_1.4.2         rjson_0.2.20         rngtools_1.4         reshape2_1.4.3      
#> [61] codetools_0.2-16     stats4_3.6.0         XML_3.98-1.20        glue_1.3.1          
#> [65] evaluate_0.14        png_0.1-7            vctrs_0.2.0          foreach_1.4.7       
#> [69] polyclip_1.10-0      gtable_0.3.0         purrr_0.3.3          tidyr_1.0.0         
#> [73] clue_0.3-57          assertthat_0.2.1     ggplot2_3.2.1        xfun_0.11           
#> [77] gridBase_0.4-7       eulerr_6.0.0         xtable_1.8-4         skmeans_0.2-11      
#> [81] survival_2.44-1.1    viridisLite_0.3.0    tibble_2.1.3         iterators_1.0.12    
#> [85] AnnotationDbi_1.46.1 registry_0.5-1       memoise_1.1.0        IRanges_2.18.3      
#> [89] cluster_2.1.0        brew_1.0-6
```


