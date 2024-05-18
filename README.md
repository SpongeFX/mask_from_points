# <img src="https://static.sidefx.com/images/apple-touch-icon.png" width="25" height="25" alt="Hbuild Logo"> Mask From Points 1.0

![license](https://img.shields.io/badge/license-MIT-green) ![version](https://img.shields.io/badge/version-1.0-blue) 

### Mask From Points 1.0 is a Houdini SideFX digital asset for point masking, based on infection simulation(transferring mask attribute between neighboring points), allowing the creation and control of mask activators on animated objects.


[![demo](https://spongefx.art/asset/media/public/simshot.mp4)](https://github.com/SpongeFX/simshot/assets/152398516/8f4dfd2c-98bf-4934-b3d4-8d5e296b0c8d)


The asset enables creating curves on polygonal objects by mouse click in the viewport and using these curves as attribute spreading activators between points. The created curves are attached to polygons and inherit all their transformations (if the polygonal object on which the curve is created moves, the curve will move along with it). Asset settings also allow controlling the speed and shape of mask spreading, smoothing the curves' shape, managing scatter to create masked points, applying parent transformations to these points, assigning the @orient attribute to points, and changing the rotation angle.


## 💾  Installation 
[Download the HDA file](otls/sop_mask_from_points.1.0.hdalc) and install it to your `houdini20/otls/` folder. For detailed instructions, please refer to the [Houdini documentation](https://www.sidefx.com/docs/houdini/assets/install.html)

## ☑️ Features

+ Tested in Houdini versions: 19.5, 20.0
+ Control and flexible adjustment of mask attribute propagation parameters.
+ Creation of various activators for mask attribute propagation within a single HDA.
+ Individual settings for created activators.
+ Works with animated polygonal objects.
+ Generates quaternion @orient to control and randomize rotation angle for each point.
  

## 🏃 Quick guide how to use the Mask From Points 1.0

<table>
    <tr>
        <td width="50%">         
            <p><b>1</b>. Connect polygonal objects to <i>input1</i>.</p>
            <p><b>2</b>. Select the asset in the Network View, activate "Show Handle," and use mouse clicks in the viewport to create curves* on the surface of objects.<br>
            *To move to creating the next curve, change the value of the Group parameter.</p>
            <p><b>3</b>. Set the value of the "<i>Carve</i>" parameter(individually for each curve) to a value more than 0.</p>
            <p><b>4</b>. Adjust parameters affecting the speed and shape of mask spreading if need.</p>
            <p><b>5</b>. Visualize to <i>output2</i> and activate the timeline.</p>
        </td>
        <td width="50%"><img src="https://github.com/SpongeFX/mask_from_points/assets/152398516/e26a8ebe-9924-43a0-a80b-f14f8698d995"></td>
    </tr>
</table>

#### Recommendations:
- If the connected primitives in input1 have an "xform" attribute with the primitive transformation matrix, turn off the "GetXFORM" toggle in the "IncomingGeometry" tab. This will cancel unnecessary computations and improve the asset's performance.

<br>

## 🔀 Description of Inputs and Outputs:

<table border="0">
    <tr>
        <td width="17%" td rowspan="2" valign="top">📥 Inputs:</td>
        <td><p><i>Input1</i> - This input is used to connect polygonal geometry for creating points on it to add mask, and curves to activate masking.</p></td>
    </tr>
    <tr>
        <td><p><i>Input2</i> - This input is used to connect points or geometric objects as sources to activate masking. More details can be found in the description of the “<i>Source</i>” tab.</p></td>
    </tr>
    <tr><td td rowspan="5" valign="top">📤 Outputs:</td>
        <td><p><i>outpout1</i> - This output returns the geometry connected to <i>input1</i> and curves created by the asset. The "source" tab provides the option to split the output result.</p></td>
    </tr>
    <tr>
        <td><p><i>outpout2</i> - This output returns masking points and the simulation result of mask spreading. The "source" tab provides the option to split the output result.</p></td>
    </tr>    
</table><br>
<a id="parm"></a>
 
## 📂 Tabs and Parameters

### 📁 Default Parameters

This tab contains the settings for simulating and creating curves. 

<table border="1">
    <tr>
        <td valign="top" width="18%"><i>Group Name</i></td>
        <td colspan="2"><p>The name of the point group where primitives are created. To finish creating the current primitive and move to the next one, change the value of this parameter.</p></td>
    </tr>
     <tr>
        <td valign="top" width="18%"><i>Reset</i></td>
        <td colspan="2"><p>Remove all created curves.</p></td>
    </tr>
    <tr>
        <td valign="top" width="18%"><i>Carve #</i></td>
        <td colspan="2"><p>The mask attribute value based on the curve length. Increasing parameter values from 0 to 1 increases the mask attribute value for points on the curve. This parameter is unique for each curve.</p></td>
    </tr>   
</table>

<br>

### 📁 Resample and Smooth Curves
In this tab, you'll find settings for adding mask activators points to created curves or geometry connected to <i>input2</i>, as well as settings for smoothing the curves created by the asset. The settings are identical to the SOP nodes "Resample" and "Smooth".

<br/>

### 📁 Scatter

This tab contains settings for adding points to geometry connected to <i>input1</i>. The settings are identical to the SOP node Scatter and differ only by the presence of one additional parameter, which adds volume and allows creating points inside objects rather than on their surface. 

<table border="1">
    <tr>
        <td valign="top" width="18%"><i>Volume From Geomerty</i></td>
        <td colspan="2"><p>Creates a volume from closed polygonal objects and uses the volume for scatter. Enabling this toggle opens the parameters for volume resolution settings identical to the "IsoOffset" SOP node.</p></td>
    </tr>
    
</table>

<br>
<a id="masking"></a> 

### 📁 Masking
This tab contains parameters affecting the speed and shape of mask spreading.

<table border="1">
    <tr>
        <td valign="top" width="18%"><a id="expmode"></a><i>Colorize</i></td>
        <td><p>Color visualization of the mask. Interpolation between red and green based on the mask attribute.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>Mask Multiplier</i></td>
        <td><p>Multiplier of the mask value. When the mask attribute value on a point  during simulation is greater than 0, then each subsequent frame increases the mask attribute by the value set in the parameter. Allows controlling the speed and time of mask propagation.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>Mask Threshold</i></td>
        <td><p>The minimum attribute value for mask propagation when searching for points. If the value is 1, then only those points that have a mask attribute value >= 1 will initiate an increase in the attribute if such points are within the search radius. This affects the speed and shape of mask propagation.</p></td>
    </tr>
    <tr>
        <td valign="top"><a id="edelay"></a><i>Diffuse Search</i></td>
        <td><p>Adds randomness to the radius for point search. Increases diffusion at the boundary of mask propagation. The minimum random value when searching for points. Values are set from 0 to 1, where 1 is the maximum value, which is the value of the "<i>Radius</i>" parameter.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>Radius</i></td>
        <td><p>The radius for point search by default. The value of this parameter determines the search radius for points to determine the presence of the mask attribute on them. This parameter differs for points to which the mask attribute was passed from neighboring points ("<i>Search Settings Between Points</i>") and points that received the attribute from activation points ("<i>Activation Points Search Settings</i>").</p></td>
    </tr>
    <tr>
        <td valign="top"><i>Max Points</i></td>
        <td><p>The maximum number of required points. This parameter differs for points to which the mask attribute was passed from neighboring points ("<i>Search Settings Between Points</i>") and points that received the attribute from activation points ("<i>Activation Points Search Settings</i>")</p></td>
    </tr>
    <tr>
        <td valign="top"><i>Uneved Border</i></td>
        <td><p>The minimum random value when searching for points. Values are set from 0 to 1, where 1 is the maximum value, which is the value of the "<i>Radius</i>" parameter. The minimum random value when searching for points. Values are set from 0 to 1, where 1 is the maximum value, which is the value of the "<i>Radius</i>" parameter. This parameter differs for points to which the mask attribute was passed from neighboring points ("<i>Search Settings Between Points</i>") and points that received the attribute from activation points ("<i>Activation Points Search Settings</i>")</p></td>
    </tr>
</table><br>
<a id="ric"></a> 

### 📁 Attributes
In this tab, you can set the attribute name for masking, control rotation angle using the quaternion @orient, and also create a duplicate of the mask attribute and modify parameter values for specific usage scenarios (such as activating growth of instanced geometry, etc.).


<table border="1">
    <tr>
        <td valign="top" width="18%"><a id="expmode"></a><i>Attribute Name</i></td>
        <td><p>The name of the masking attribute.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>Duplicate Attribute</i></td>
        <td><p>Creates a duplicate of the masking attribute.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>Attribute Name</i></td>
        <td><p>The name of the duplicate masking attribute.</p></td>
    </tr>
    <tr>
        <td valign="top"><a id="edelay"></a><i>Base Value</i></td>
        <td><p>The value from which the masking attribute will start increasing. Use this if you need the default masking attribute to be below 0.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>Value Multiplayer</i></td>
        <td><p>Multiplier for the values of the duplicate masking attribute. Use this if you need to increase the attribute value with greater or lesser intensity relative to the original masking attribute values.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>Copy Normal To Points</i></td>
        <td><p>Copying @N attribute from primitives to points.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>Randomize Rotation</i></td>
        <td><p>Randomizes rotation angle values. Use this if you want to randomize the rotation angle for all points.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>Min Random</i></td>
        <td><p>Min randomization angle</p></td>
    </tr>
    <tr>
        <td valign="top"><i>Max Random</i></td>
        <td><p>Max randomization angle.</p></td>
    </tr>
</table><br>


### 📁 Source 	 
Here you can enable the application of points or geometry from <i>input2</i> as masking activators. This tab also includes settings for splitting output results.

<table border="1">
    <tr>
        <td valign="top" width="18%"><i>Import Source</i></td>
        <td><p>Use this if you already have points or primitives to initialize the masking and there is no need to create curves on the surface of objects.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>Source Type</i></td>
        <td><p>Type of imported geometry source. If set to "primitives," resample and smooth also apply to them if they are enabled. The masking attribute for all points (imported or created on the surface of primitives) has a value of 1.0.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>Output1 Geometry</i></td>
        <td><p>Output 1 options. "Only input1 geo" - only the geometry connected to input1 of the asset is output in <i>output1</i>. "Only curves" - only curves created on the surface of the geometry asset. "Both" - both the geometry connected to input1 and the curves created on the surface of the geometry using the asset are output.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>Output2 Geometry</i></td>
        <td><p>Output 2 options. "Only masked Points" - only points with a mask attribute greater than 0 are output to output 1. "Only non-masked points" - with a mask attribute equal to 0. "Both" - all points.</p></td>
    </tr>
</table><br>

### 📁 Incomming Geomerty

This tab contains settings necessary to obtain the transformation matrix from incoming polygons. Use this if your geometry does not contain a primitive attribute <i>@xform</i> with a transformation matrix4.

<table border="1">
    <tr>
        <td valign="top" width="18%"><a id="expmode"></a><i>Get XFORM</i></td>
        <td><p>Enabling activates the calculation of the transformation matrix4 using a faster but less efficient method. This works well with objects that do not deform (do not change their sizes) over time. For proper operation, specify the attribute name containing the object identifier in the "<i>Attribute name" parameter</i>.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>Get XFORM for each primitive</i></td>
        <td><p>Enabling activates the calculation of the transformation matrix4 for each primitive. This method works with deforming objects. It is a more efficient method but may take more time. For proper operation, specify the attribute name containing the object identifier in the "<i>Attribute name" parameter</i>.</p></td>
    </tr>
    <tr>
        <td valign="top"><i>Pieces Name</i></td>
        <td><p>Сreating an attribute containing object identifier.</p></td>
    </tr>
</table><br>

## 💬 Feedback

If you have any feedback or run into issues, please feel free to open an issue on this GitHub project.
