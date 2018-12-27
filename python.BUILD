# Description:
#   Build rule for Python and Numpy.
#
#	NEW
#	---
#   python headers /Users/mtrazzi/.brew/Cellar/python@2/2.7.15_1/Frameworks/Python.framework/Headers
#   numpy get include with the python from Cellar /Users/mtrazzi/Library/Python/2.7/lib/python/site-packages/numpy/core/include/numpy/*
#
#   OLD
#   ---
#
#     "python@2/2.7.15/Frameworks/Python.framework/Versions/2.7/include/python2.7/*.h",
#     "numpy/1.15.2/lib/python2.7/site-packages/numpy/core/include/numpy/*.h",
# ]),
# includes = [
#     "python@2/2.7.15/Frameworks/Python.framework/Versions/2.7/include/python2.7",
#     "numpy/1.15.2/lib/python2.7/site-packages/numpy/core/include",
# ],

cc_library(
    name = "python",
    hdrs = glob([
        ".brew/Cellar/python@2/2.7.15_1/Frameworks/Python.framework/Headers/*.h",
        "Library/Python/2.7/lib/python/site-packages/numpy/core/include/numpy/*.h",
    ]),
    includes = [
        ".brew/Cellar/python@2/2.7.15_1/Frameworks/Python.framework/Headers",
        "Library/Python/2.7/lib/python/site-packages/numpy/core/include",
    ],
    visibility = ["//visibility:public"],
)
