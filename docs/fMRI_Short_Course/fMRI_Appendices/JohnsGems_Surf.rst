.. _JohnsGems_Surf:

This is code adapted from `John's Gems <https://warwick.ac.uk/fac/sci/statistics/staff/academic-research/nichols/scripts/spm/johnsgems/>`__. I use it to demonstrate how cluster correction works.


V=spm_vol('zstat1.nii.gz'); % Load the zstat file into memory

M=spm_matrix([0 0 28]) % Select the 28th slice in the z-direction
img=spm_slice_vol(V,M,V.dim(1:2),1);
img(img==0)=NaN; % Remove the 0s to make the background uniformly black
surf(img); % Create the surface map of the statistical image

set(gca, 'Color', 'k') % Set the background to black
axis off % And turn off the axes

%%% Extra options, not needed %%%

set (gca, 'FontSize', 20)
zlabel('Z-value', 'FontSize', 15, 'Fontweight', 'bold')
