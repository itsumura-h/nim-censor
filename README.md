# nim-censor
Full-stack test framework for Nim based on testament

## Motivation
I have some complaints for testament. One is that we have to creat test file for each test suits.  
However, in my opinion, `spec` should be able to written inside of block.

## Design

tests/sample.nim
```nim
import
  strutils,
  strformat,

suite "sample suit1":
  setup:
    discard "this is set up"
    
  test "test1":
    discard """
      output: "test1"
    """
    echo "test1"
     
  test "test2":
    discard """
      input: "hello world"
    """
    doAssert stdin.readline == "hello world"
```

↓ Compile to like this

tests_censor/sample/sample_suit1/test1.nim
```nim
discard """
  output: "test1"
"""

import
  strutils,
  strformat,

block:
  discard "this is set up"

block:
  echo "test1"
```

tests_censor/sample/sample_suit1/test2.nim
```nim
discard """
  input: "hello world"
"""

import
  strutils,
  strformat,
  
block:
  discard "this is set up"

block:
  doAssert stdin.readline == "hello world"
```

Then run test
```sh
testament p tests_censor/**/**/*.nim
```
