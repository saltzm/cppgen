# cppgen
A tool for generating C++ .h, .cpp, and test file from a short class description.

An example definition file can be seen in ExampleClass.js.dna (copied here for reference). You can define as 
many classes as you want in the definition file and files for each of them will be 
generated when you run the tool.

```js
// Classes
var def = {
    ExampleClass : {
        namespace : "mongo",
        // The directory inside the "src" directory where the files will
        // go. Make sure to include the trailing "/".
        directory : "mongo/db/s/",
        // Whether or not an object of this class should be copyable.
        // Affects the declaration and definition of copy constructors.
        copyable : true,
        // Whether or not an object of this class should be moveable.
        // Affects the declaration and definition of move constructors.
        moveable : true,
        // If false, marks the class final. If true, marks the destructor 
        // virtual.
        virtual : false,
        // Which log component the class will use.
        logComponent : "Sharding",
    },
    // You can create as many classes as you want by listing more here
    // ExampleClass2 : {
    //    namespace : "mongo",
    //    directory : "mongo/util/",
    //    copyable : false,
    //    moveable : true,
    //    virtual : false,
    //    logComponent : "Sharding",
    // },
}

./!include("cppgen.js.dna")
```

This was hacked together quickly and will likely be refined overtime. Code is
generated using the [ribosome tool](http://sustrik.github.io/ribosome/index.html).
The source for this tool, ribosome.js, is included here for convenience.

This requires node to be installed on your machine.

To use, clone this repo, and copy the files ribosome.js, cppgen.js.dna, and
copyright.js.dna into the root directory of your checkout of the mongo repo.

**WARNING: When running the tool, there is currently no check to see if the 
files exist or not. If you regenerate a file, it will overwrite any existing 
files, so be careful!!!**

To run the tool with the example given, copy ExampleClass.js.dna into the root
directory of your checkout of the mongo repo and run:
```
$ node ribosome.js ExampleClass.js.dna


Generating src/mongo/db/s/example_class.h...
Generating src/mongo/db/s/example_class.cpp...
Generating src/mongo/db/s/example_class_test.cpp...

IMPORTANT: Remember to add your .cpp file to a library in src/mongo/db/s/SConscript,
or create a new one. Also add your unit test to a CppUnitTest definition.
Creating new libraries just for your new files is NOT recommended for
production, but may be helpful for getting started.

# Defines a new library for your class. Note that this is not recommended,
# and that you should probably add your cpp file to an existing library.
env.Library(
    target='example_class',
    source=[
        'example_class.cpp',
    ],
    LIBDEPS=[
        '$BUILD_DIR/mongo/base',
        # TODO other libraries you depend on
    ],
)

# Defines a new unit test target, runnable as ninja +example_class_test
env.CppUnitTest(
    target='example_class_test',
    source=[
        'example_class_test.cpp',
    ],
    LIBDEPS=[
        # Change to correct library name that contains your cpp file
        'example_class',
    ]
)
```

It will generate the following files:

### src/mongo/db/s/example\_class.h: 

```cpp
/**
 *    Copyright (C) 2018 MongoDB Inc.
 *
 *    This program is free software: you can redistribute it and/or  modify
 *    it under the terms of the GNU Affero General Public License, version 3,
 *    as published by the Free Software Foundation.
 *
 *    This program is distributed in the hope that it will be useful,
 *    but WITHOUT ANY WARRANTY; without even the implied warranty of
 *    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
 *    GNU Affero General Public License for more details.
 *
 *    You should have received a copy of the GNU Affero General Public License
 *    along with this program.  If not, see <http://www.gnu.org/licenses/>.
 *
 *    As a special exception, the copyright holders give permission to link the
 *    code of portions of this program with the OpenSSL library under certain
 *    conditions as described in each individual source file and distribute
 *    linked combinations including the program with the OpenSSL library. You
 *    must comply with the GNU Affero General Public License in all respects for
 *    all of the code used other than as permitted herein. If you modify file(s)
 *    with this exception, you may extend this exception to your version of the
 *    file(s), but you are not obligated to do so. If you do not wish to do so,
 *    delete this exception statement from your version. If you delete this
 *    exception statement from all source files in the program, then also delete
 *    it in the license file.
 */

#pragma once

// TODO: System header includes here

// TODO: mongo header includes here


namespace mongo {

class ExampleClass final {
public:
    ExampleClass(/*TODO: Parameters here*/);
    ~ExampleClass();

    ExampleClass(const ExampleClass &other);
    ExampleClass& operator=(const ExampleClass &other);

    ExampleClass(ExampleClass &&source) noexcept;
    ExampleClass& operator=(ExampleClass &&other) noexcept;

private:
   // TODO: Data members here
};

} //namespace mongo
```

### src/mongo/db/s/example\_class.cpp: 

```cpp
/**
 *    Copyright (C) 2018 MongoDB Inc.
 *
 *    This program is free software: you can redistribute it and/or  modify
 *    it under the terms of the GNU Affero General Public License, version 3,
 *    as published by the Free Software Foundation.
 *
 *    This program is distributed in the hope that it will be useful,
 *    but WITHOUT ANY WARRANTY; without even the implied warranty of
 *    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
 *    GNU Affero General Public License for more details.
 *
 *    You should have received a copy of the GNU Affero General Public License
 *    along with this program.  If not, see <http://www.gnu.org/licenses/>.
 *
 *    As a special exception, the copyright holders give permission to link the
 *    code of portions of this program with the OpenSSL library under certain
 *    conditions as described in each individual source file and distribute
 *    linked combinations including the program with the OpenSSL library. You
 *    must comply with the GNU Affero General Public License in all respects for
 *    all of the code used other than as permitted herein. If you modify file(s)
 *    with this exception, you may extend this exception to your version of the
 *    file(s), but you are not obligated to do so. If you do not wish to do so,
 *    delete this exception statement from your version. If you delete this
 *    exception statement from all source files in the program, then also delete
 *    it in the license file.
 */

#define MONGO_LOG_DEFAULT_COMPONENT ::mongo::logger::LogComponent::kSharding

#include "mongo/platform/basic.h"

#include "mongo/db/s/example_class.h"

// TODO: System includes go here

// TODO: mongo includes go here
#include "mongo/util/log.h"

namespace mongo {

namespace {
//TODO Helper functions and constants go here
} //namespace

ExampleClass::ExampleClass() = default;

ExampleClass::~ExampleClass() = default;

ExampleClass::ExampleClass(const ExampleClass &other) = default;

ExampleClass& ExampleClass::operator=(const ExampleClass &other) = default;

ExampleClass::ExampleClass(ExampleClass &&source) noexcept = default;

ExampleClass& ExampleClass::operator=(ExampleClass &&other) noexcept = default;

} //namespace mongo
```

### src/mongo/db/s/example\_class\_test.cpp: 

```cpp
/**
 *    Copyright (C) 2018 MongoDB Inc.
 *
 *    This program is free software: you can redistribute it and/or  modify
 *    it under the terms of the GNU Affero General Public License, version 3,
 *    as published by the Free Software Foundation.
 *
 *    This program is distributed in the hope that it will be useful,
 *    but WITHOUT ANY WARRANTY; without even the implied warranty of
 *    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
 *    GNU Affero General Public License for more details.
 *
 *    You should have received a copy of the GNU Affero General Public License
 *    along with this program.  If not, see <http://www.gnu.org/licenses/>.
 *
 *    As a special exception, the copyright holders give permission to link the
 *    code of portions of this program with the OpenSSL library under certain
 *    conditions as described in each individual source file and distribute
 *    linked combinations including the program with the OpenSSL library. You
 *    must comply with the GNU Affero General Public License in all respects for
 *    all of the code used other than as permitted herein. If you modify file(s)
 *    with this exception, you may extend this exception to your version of the
 *    file(s), but you are not obligated to do so. If you do not wish to do so,
 *    delete this exception statement from your version. If you delete this
 *    exception statement from all source files in the program, then also delete
 *    it in the license file.
 */

#include "mongo/platform/basic.h"

#include "mongo/db/s/example_class.h"

// TODO: System includes go here

// TODO: Other mongo includes go here
#include "mongo/unittest/death_test.h"
#include "mongo/unittest/unittest.h"

namespace mongo {
namespace {

class ExampleClassTest : public unittest::Test {
public:
    void setUp() override {}
    void tearDown() override {}
private:
};

// Example test:
TEST_F(ExampleClassTest, MyFirstTest) {
    ASSERT_EQ(true, false);
}

} //namespace
} //namespace mongo
```
