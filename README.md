# colcon_lcov
An extension for colcon-core to provide aggregate coverage results using LCOV.

## configure project

## how to get code coverage report
* Build your packages with coverage flags, using colcon:
```bash
colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Debug -DCOVERAGE_ENABLED=True
colcon test
```
**Note**:Please run this script from the root of your workspace.
* Generate initial zero-coverage data. This adds files that were otherwise not run to the report
```bash
lcov -c -b ./src/ros2_openvino_toolkit --initial --rc lcov_branch_coverage=1 --directory build --output-file ${LCOVDIR}/initialcoverage.info
```
* Capture executed code data.
```bash
lcov -c -b ./src/ros2_openvino_toolkit --rc lcov_branch_coverage=1 --directory build --output-file ${LCOVDIR}/testcoverage.info
```
* Combine the initial zero-coverage report with the executed lines report
```bash
lcov -a ${LCOVDIR}/initialcoverage.info -a ${LCOVDIR}/testcoverage.info --rc lcov_branch_coverage=1 --o ${LCOVDIR}/fullcoverage.info
```
* Only include files that are within this workspace (eg filter out stdio.h etc)
```bash
lcov -e ${LCOVDIR}/fullcoverage.info "${PWD}/*" --rc lcov_branch_coverage=1 --output-file ${LCOVDIR}/workspacecoverage.info
```
* Remove files in the build subdirectory because they are generated files (like messages, services, etc)
```bash
lcov -r ${LCOVDIR}/workspacecoverage.info "${PWD}/build/*" --rc lcov_branch_coverage=1 --output-file ${LCOVDIR}/projectcoverage.info
lcov -r ${LCOVDIR}/workspacecoverage.info "${PWD}/build/*" "${PWD}/src/image_common" "${PWD}/src/message_filters" "${PWD}/src/ros2_intel_realsense" "${PWD}/src/ros2_object_msgs" "${PWD}/src/vision_opencv" --rc lcov_branch_coverage=1 --output-file ${LCOVDIR}/projectcoverage.info
```
* generate the html
```bash
 genhtml ${LCOVDIR}/projectcoverage.info --output-directory ${LCOVDIR}/html --branch-coverage -p ${PWD}
```
