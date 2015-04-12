# Grasp

Robot manipulation learning framework.

Installation
------------

Installing dependencies for **Grasp** package:

* Update database `sudo apt-get update`
* [GolemCore](https://github.com/marekkopicki/Golem) package.
* Boost libraries: `sudo apt-get install libboost-filesystem-dev`.
* OpenCV libraries: `sudo apt-get install libopencv-dev libopencv-highgui-dev libcvaux-dev`
* PCL libraries (the default installation, currently PCL 1.7): `sudo add-apt-repository ppa:v-launchpad-jochen-sprickerhof-de/pcl`, `sudo apt-get update`, then run `sudo apt-get install libpcl-all`.

Grasp Debian package has been created and tested on Ubuntu 14.04 LTS.


GraspPlayer demonstration
-------------------------

Download grasp data for robot Boris.

* `mkdir ObjectDB`
* `git clone https://github.com/marekkopicki/ObjectDB-2014-Boris-v7 ObjectDB`

Run application with robot Boris configuration. Make sure you have write permissions in the current directory.

* `GraspPlayer /usr/bin/GraspPlayer_RobotBoris.xml`
* Press `?` for a brief help.

Load grasp data.

* Press `D`, `L` to enter load menu.
* Enter data bundle file `Enter data path or directory to load: ./ObjectDB/models.xml`, use `<tab>` key for quick selection.
* Data bundles can be selected using `{` and `}` keys.
* Data items in the current data bundle can be selected using `[` and `]` keys.

Create training data.

* Use data item transformation interface: press `I`, `T`.
* Select data handler: `Handler: ContactModel+ContactModel, Item types: PointsCurv Trajectory`.
* Enter non-empty training data item label, for example `A`.
* Select input items of `PointsCurv` and `Trajectory` types: `(bowllarge-curv-all_views/DEFAULT, 15) (bowllarge-pinchsupp/DEFAULT, 16)` and press `<enter>`.
* `Enter grasp type: pinchsupp`.
* Training data for a single grasp type is created: ![Training data](/docs/training_pinchsupp.png).

Test on a point cloud (create test data).

* Use data item transformation interface: press `I`, `T`.
* Select data handler: `Handler: ContactQuery+ContactQuery, Item types: ContactModel PointsCurv`.
* Enter non-empty test data item label, for example `A`.
* Select input items of `PointsCurv` and `ContactModel` types: `(A/DEFAULT, 1) (mug2-curv-all_views/DEFAULT, 49)`.
* Select grasp type: `all`.
* Test data is created: ![Test data](/docs/test_pinchsupp.png).
* Solutions can be selected using '(' and ')'.

Find feasible trajectory.

* Select `ContactQuery` item using '[' and ']' keys.
* Select initial solution using '(' and ')'.
* Use item convert interface: press `I`, `C`.
* Select data handler: `Handler: Trajectory+Trajectory`.
* Enter non-empty trajectory data item label, for example `A`.
* Enter number of trajectories to test.
* Trajectory can be traversed using mouse wheel (also with `Ctrl` key).

Execute trajectory

* Enter trajectory menu: press `T`.
* Play trajectory: press `P`.
* Enter trajectory extrapolation factor and duration.
* Confirm additional fixed action at the end of the trajectory (object lifting by default).
* Select an object to be used for collision avoidance using '[' and ']' keys. Here it should be `mug2-curv-all_views/DEFAULT, item 51`.
* Confirm trajectory by pressing `Enter`, or cancel by pressing `Esc`.
* If confirmed, new items `A-?` will be created - executed trajectory and video streams from cameras which have been registered for recording.


GraspTiny interface
-------------------

GraspTiny interface is a simple C++ interface to the Grasp and Golem frameworks specified only by a single header file `Tiny.h`. GraspTiny interface is demonstrated in `GraspTiny.cpp` built as `GraspDemoGraspTiny` with a configuration file `GraspTiny.xml`:

* `GraspDemoGraspTiny /usr/bin/GraspTiny.xml`

`GraspDemoGraspTiny` demonstrates all the training, test and execution stages as described in the GraspPlayer demonstration, however performed in a sequence of TinyGrasp interface calls.
