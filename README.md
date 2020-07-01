# Music Structure Analysis in Matlab
Implememtation on two segmentation and one labeling algorithms in matlab. Additionally, it contains a toolbox and a workspace for facilitating coding.  
音樂分段和標籤演算法的實作

**NEW** pyhton implementation is [here](https://github.com/wayne391/sf_segmenter).

Related Topics: Segmentation, Lableing, Recurrence Plot (RP), Self-Similarity Matrix(SSM) 

  
## Dependencies
* Chroma Toolbox (matlab toolbox) [4] : [http://resources.mpi-inf.mpg.de/MIR/chromatoolbox/](http://resources.mpi-inf.mpg.de/MIR/chromatoolbox/)
* mir_eval (python package) [5] : For evaluation (Optional)
* madmom [7] : For HPCP, DCP chroma feature (Optional)

Note that there are a warning in origirnal Chroma Toolbox and a little bug that it can't read .mp3. I fixed them!
## Usage
There are two folders<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|- segmentaion toolbox/ : set path and it can be used directly   
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|- workspace/ : a template for testing and evaluating a dataset

### segmentaion_toolbox/
Adding this folder to toolbox or addpath, and it's easy to use.
```matlab
% Segmentation
audio_filename = 'test.wav';
result = audio_segmenter_sf(audio_filename);
visualize_results(audio_filename, result);

% Segmentation & Labeling
[result_sf, labeling] = audio_segmenter_sf(audio_filename,'clp', 0, 1);
```

see demo.m for further using
  
### workspace/  
If you want to use this template, please follows the structure of folders.  
  
root/    
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|- annotation/ : groundtruth or anntations files  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|- audio/ : audio files  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|- estimation/ : results of the program     
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|- feature/ : generated features  
  
In the root folder (workspace here), there are three programs. Following the procedures, you can experiments on a dataset.  
  
1. run "feature_saving.m" and generated features  will be placed at the feature folder  
2. run "run_all.m" and the results of prediction will be palced at estimation folder  
3. run "eval.py" to see the performance. (Optional)
4. if needed, run "hpcp_dcp.py" to gain hpcp and dcp chorma feature

Note that the amount of annotation files will dominate the amount of evaluation. To see details in "run_all.m" and "eval.py".
Note that there are existing results in estimation folder, the parameter is default (see below).   
Note that for the reason of copyright, I won't put any audio files here.

## Algorithms, Features & Performance
### Algorithms
* Segmentation  
&nbsp;&nbsp;&nbsp;&nbsp;1. Structure Feature (2012) (default) [1]        
&nbsp;&nbsp;&nbsp;&nbsp;2. Checkboard Kernel (2000) [2]   
* Labeling  
&nbsp;&nbsp;&nbsp;&nbsp;1. Structure Feature (2014) [6]

Generally, it's recommended to use the first one - "Structure Feature". It's still one of most effective segmentation algorithms. However, Checkboard Kernel is simple to implement :).  
### Features
From Chroma Toolbox: CLP (default), CENS, CRP
From Madmom: HPCP, DCP

To see the influence on performance of chroma feature, please refer to [3] 
Note that there are no MFCC feature, but my function accept customized feature  as input.
### Performance
* dataset:   
&nbsp;&nbsp;&nbsp;&nbsp;Beatles (174 songs)  
* parameters (default):  
&nbsp;&nbsp;&nbsp;&nbsp;Chroma Feature: winLenSTMSP = 4410  
&nbsp;&nbsp;&nbsp;&nbsp;Structure Feature (SF): (m, k, st) = (2.5, 0.04, 30)  
&nbsp;&nbsp;&nbsp;&nbsp;Checkboard Kernel (foote): winLen = 64
* evaluation (by mir_eval):  
&nbsp;&nbsp;&nbsp;&nbsp;Segmentaion (Seg): F-measure with 3s tolerance  
&nbsp;&nbsp;&nbsp;&nbsp;Labeling (Lab): Pairwise Precision  
  
| Algo          | Feature       | Seg      | Lab     |
| ------------- |:-------------:| --------:|--------:|
| SF            | CENS          | 0.711    | 0.692   |
|               | CLP           | 0.695    | 0.660   |
|               | CRP           | 0.694    | 0.650   |
|               | HPCP          | 0.630    | 0.600   |
|               | HPCP          | 0.689    | 0.645   |
| Foote         | CENS          | 0.448    | --      |
|               | CLP           | 0.440    | --      |
|               | CRP           | 0.423    | --      |

I think the performance of foote is not good enough. Maybe somewhere is wrong.

## References
1. Serrà, J., Müller, M., Grosche, P., & Arcos, J. L. (2012). Unsupervised Detection of Music Boundaries by Time Series Structure Features. In Proc. of the 26th AAAI Conference on Artificial Intelligence (pp. 1613–1619).Toronto, Canada.  
2. Foote, J. (2000). Automatic Audio Segmentation Using a Measure Of Audio Novelty. In Proc. of the IEEE International Conference of Multimedia and Expo (pp. 452–455). New York City, NY, USA.  
3. Nieto, O., Bello, J. P., Systematic Exploration Of Computational Music Structure Research. Proc. of the 17th International Society for Music Information Retrieval Conference (ISMIR). New York City, NY, USA, 2016.
4. Meinard Müller and Sebastian Ewert Chroma Toolbox: MATLAB Implementations for Extracting Variants of Chroma-Based Audio Features Proceedings of the International Conference on Music Information Retrieval (ISMIR), 2011.
5. Colin Raffel, Brian McFee, Eric J. Humphrey, Justin Salamon, Oriol Nieto, Dawen Liang, and Daniel P. W. Ellis, "mir_eval: A Transparent Implementation of Common MIR Metrics", Proceedings of the 15th International Conference on Music Information Retrieval, 2014.
6. Joan Serra, Meinard M ` uller, Peter Grosche, and Josep Llu´ıs Arcos. Unsupervised Music Structure Annotation by Time Series Structure Features and Segment Similarity.
7. [http://resources.mpi-inf.mpg.de/MIR/chromatoolbox/] (https://github.com/CPJKU/madmom)
