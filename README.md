# periodic-point-cloud
C++ library that generates periodic point clouds spanned by a set of vectors or generators

## Installation
```
cmake -B build -S . -L -DCMAKE_INSTALL_PREFIX=install
cmake --build build --target install --config Release
```

## Usage
```
// Extend each vector n times to generate more periodic points
unsigned int n = 5;
std::vector<Eigen::Vector3d> generators = {
    Eigen::Vector3d(1,0,0),
    Eigen::Vector3d(0,1,0),
    Eigen::Vector3d(0,0,1)
}
PeriodicPointCloud ppc(generators.size());
for (auto& g : generators) {
    ppc.addGenerator(std::move(g));
}
ppc.updateCoefficientsForPositiveDirection(n);
ppc.updateCoefficientCombinations();
```
Then, if you want to span the periodic point cloud from the generators 
and integer coefficients only call the following with no parameters:
```
ppc.updateGeneratorCombinations();
```
otherwise, create linear combinations from any vector collection
```
std::vector<Eigen::Vector3d> points = {
    Eigen::Vector3d(0.5,0.5,0.5),
    Eigen::Vector3d(0.3,0.3,0.3)
}
ppc.updateGeneratorCombinationsForPoints(points);
```
and get the entire point cloud in the following way
```
std::vector<Eigen::Vector3d> pointcloud = ppc.getPointCloud();
```

## Tests
```
cmake -B build -S . -L -DCMAKE_INSTALL_PREFIX=install
cmake --build build --target install --config Debug
cd build
ctest --build-config Debug --build-target install
```
