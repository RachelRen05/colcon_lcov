# colcon-lcov-result
An extension for colcon-core to provide aggregate coverage results using LCOV.

LCOV is a graphical front-end for GCC's coverage testing tool gcov, producing the following coverage metrics:

  * Statement coverage
  * Function coverage
  * Branch coverage

For more information, see this paper and this Wikipedia page.

# usage
* Install colcon-lcov-result
```bash
git clone https://github.com/colcon/colcon-lcov-result.git
cd colcon-lcov-result
pip3 install setup.py
```
* Build your packages with coverage flags, using colcon:
```bash
colcon build --symlink-install --cmake-args -DCMAKE_CXX_FLAGS='-fprofile-arcs -ftest-coverage' -DCMAKE_C_FLAGS='-fprofile-arcs -ftest-coverage'
```
* Create a baseline for zero coverage:
```bash
colcon lcov-result --initial
```
* Run tests
```bash
colcon test
```
* Gather the lcov results
```bash
colcon lcov-result --packages-select dynamic_vino_lib dynamic_vino_sample vino_param_lib
```
