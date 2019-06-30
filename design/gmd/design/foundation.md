# Material Design -> Foundation

## Environment

### Surfaces
Material Design has three-dimensional qualities that are reflected in its use of surfaces, depth, and shadows.

#### Depth 
MUI expresses three-dimensional (3D: x,y,z) space using light, surfaces, and cast shadows.

#### Properties
- **Dimensions**: varying x & y dimensions (measured in dp) and a uniform thickness (1dp z).
- **Shadows**: surfaces at different elevations (~z) cast shadows.
- **Resolution**: infinite.
- **Content**: Content does not add thickness to Material. Content is expressed without being a separate layer.

##### Physical properties of Material
- is solid. User input and interaction cannot pass through material.
- Multiple Material elements cannot occupy the same point in space simultaneously.
- cannot pass through other Material. For example, one Material surface cannot pass through another Material surface when changing elevation.
- does not behave like a gas; enters and exits through changes in opacity, size, or position.
- is not fluid
- can change shape and opacity.
- grows and shrinks only along its plane.
- bends or folds within the depth of the UI.
- When split, Material can rejoin

##### Movement 
- can be spontaneously generated or dismissed anywhere in the environment.
- can move along any axis
- Material surfaces can coordinate their motion
- Material motion along the z-axis is typically a result of user interaction
- 

#### Attributes
https://material.io/design/environment/surfaces.html#attributes


