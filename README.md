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
for (auto& g : testCase.generators) {
    ppc.addGenerator(std::move(g));
}
ppc.updateCoefficientsForPositiveDirection(n);
ppc.updateCoefficientCombinations();
ppc.updateGeneratorCombinations();
```

## Tests
```
cmake -B build -S . -L -DCMAKE_INSTALL_PREFIX=install
cmake --build build --target install --config Debug
cd
ctest --build-config Debug --build-target install
```
