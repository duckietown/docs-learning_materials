
### Problem

The VO problem is modelled as a sequence of transformations

<figure class="stretch">
<img src="figures/vo_scheme.png"/>
<figcaption>Figure 3.1 VO Problem Model </figcaption>
</figure>


### Input     
Image Sequence

* Monocular : I_{0:n}  = { I_0, I_1, …, I_n }       
* Stereo    : I_{l, 0:n}  = { I_{l,0}, I_{l,1}, …, I_{l,n} }
              I_{r, 0:n}  = { I_{r,0}, I_{r,1}, …, I_{r,n} }   

### Output
Camera Poses
                
Poses of the camera w.r.t. The initial frame at k = 0 

C 0:n = {C0, C1, …, Cn}  
 	     
Modelled by relative transformations T1:n
                
                T1:n = { T1,0, T2,1, …, Tn,(n-1)}      

For simplicity, use Tk, k-1 as Tk      
                              
                              
Rk, k-1 = 3D Rotational Matrix
                                                                                                                             Tk, k-1 = 3D Translation Vector              
Concatenate to recover full trajectory  :    
Cn = Cn-1 Tn                                      
             
             
             
In short : Estimate "relative motion" from image sequence   
