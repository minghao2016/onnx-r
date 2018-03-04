# ONNX-R

R interface to [ONNX](https://github.com/onnx). This package is under heavy development.

## Installation

* Follow instructions [here](https://github.com/onnx/onnx#installation) to install the onnx Python package.
* Install the development version of the R package via

```
devtools::install_github("terrytangyuan/onnx-r")
```

## Examples

We'll make use of the following functions for the examples:

* `make_xxx()` to make different types of protobufs for attributes, nodes, graphs, etc.
* a single `check()` method that can check whether a protobuf in a particular type is valid.

Define a node protobuf and check whether it's valid:

```r
library(onnx)

node_def <- make_node("Relu", list("X"), list("Y"))
check(node_def)
```

```r
> node_def
input: "X"
output: "Y"
op_type: "Relu"
```

Define an attribute protobuf and check whether it's valid:

```r
attr_def <- make_attribute("this_is_an_int", 123L)
check(attr_def)
```

```r
> attr_def
name: "this_is_an_int"
i: 123
type: INT
```

Define a graph protobuf and check whether it's valid:

```r
graph_def <- make_graph(
  nodes = list(
    make_node("FC", list("X", "W1", "B1"), list("H1")),
    make_node("Relu", list("H1"), list("R1")),
    make_node("FC", list("R1", "W2", "B2"), list("Y"))
  ),
  name = "MLP",
  inputs = list(
    make_tensor_value_info('X' , onnx$TensorProto$FLOAT, list(1L)),
    make_tensor_value_info('W1', onnx$TensorProto$FLOAT, list(1L)),
    make_tensor_value_info('B1', onnx$TensorProto$FLOAT, list(1L)),
    make_tensor_value_info('W2', onnx$TensorProto$FLOAT, list(1L)),
    make_tensor_value_info('B2', onnx$TensorProto$FLOAT, list(1L))
  ),
  outputs = list(
    make_tensor_value_info('Y', onnx$TensorProto$FLOAT, list(1L))
  )
)
check(graph_def)
```

```r
> graph_def
node {
  input: "X"
  input: "W1"
  input: "B1"
  output: "H1"
  op_type: "FC"
}
node {
  input: "H1"
  output: "R1"
  op_type: "Relu"
}
node {
  input: "R1"
  input: "W2"
  input: "B2"
  output: "Y"
  op_type: "FC"
}
name: "MLP"
input {
  name: "X"
  type {
    tensor_type {
      elem_type: FLOAT
      shape {
        dim {
          dim_value: 1
        }
      }
    }
  }
}
input {
  name: "W1"
  type {
    tensor_type {
      elem_type: FLOAT
      shape {
        dim {
          dim_value: 1
        }
      }
    }
  }
}
input {
  name: "B1"
  type {
    tensor_type {
      elem_type: FLOAT
      shape {
        dim {
          dim_value: 1
        }
      }
    }
  }
}
input {
  name: "W2"
  type {
    tensor_type {
      elem_type: FLOAT
      shape {
        dim {
          dim_value: 1
        }
      }
    }
  }
}
input {
  name: "B2"
  type {
    tensor_type {
      elem_type: FLOAT
      shape {
        dim {
          dim_value: 1
        }
      }
    }
  }
}
output {
  name: "Y"
  type {
    tensor_type {
      elem_type: FLOAT
      shape {
        dim {
          dim_value: 1
        }
      }
    }
  }
}
```
