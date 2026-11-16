# Issue

**Title:** Make TorchMD_Net always return two tensors

## Description

Currently this test always fails:

```python
def test_torchscript_output_modification():
    model = create_model(load_example_args("tensornet", remove_prior=True, derivative=True))
    class MyModel(torch.nn.Module):
        def __init__(self):
            super(MyModel, self).__init__()
            self.model = model
        def forward(self, z, pos, batch):
            y, neg_dy = self.model(z, pos, batch=batch)
            # A TorchScript bug is triggered if we modify an output of model marked as Optional[Tensor]
            return y, 2*neg_dy
    torch.jit.script(MyModel())
```
This was uncovered in  .
<details>

<summary>The error is this</summary>

```shell
platform linux -- Python 3.11.6, pytest-7.4.3, pluggy-1.3.0 -- /home/raul/miniforge3/envs/torchmdnet/bin/python3.11
cachedir: .pytest_cache
rootdir: /home/raul/work/bcn/torchmd-net
plugins: typeguard-2.13.3, anyio-3.7.1, cov-4.1.0
collected 94 items / 93 deselected / 1 selected                                                                                                                             

test_model.py::test_torchscript_output_modification FAILED

================================================================================= FAILURES ==================================================================================
___________________________________________________________________ test_torchscript_output_modification ____________________________________________________________________

    def test_torchscript_output_modification():
        z, pos, batch = create_example_batch()
        model = create_model(load_example_args("tensornet", remove_prior=True, derivative=True))
        class MyModel(torch.nn.Module):
            def __init__(self):
                super(MyModel, self).__init__()
                self.model = model
            def forward(self, z, pos, batch):
                y, neg_dy = self.model(z, pos, batch=batch)
                # A TorchScript bug is triggered if we modify an output of model marked as Optional[Tensor]
                return y, 2*neg_dy
>       torch.jit.script(MyModel())

test_model.py:75: 
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _
/home/raul/miniforge3/envs/torchmdnet/lib/python3.11/site-packages/torch/jit/_script.py:1324: in script
    return torch.jit._recursive.create_script_module(
/home/raul/miniforge3/envs/torchmdnet/lib/python3.11/site-packages/torch/jit/_recursive.py:559: in create_script_module
    return create_script_module_impl(nn_module, concrete_type, stubs_fn)
/home/raul/miniforge3/envs/torchmdnet/lib/python3.11/site-packages/torch/jit/_recursive.py:636: in create_script_module_impl
    create_methods_and_properties_from_stubs(
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

concrete_type = <torch.ConcreteModuleType object at 0x7f83ba404970>
method_stubs = [ScriptMethodStub(resolution_callback=<function createResolutionCallbackFromEnv.<locals>.<lambda> at 0x7f83b1e5c9a0>, ..., bias=True)
        (1): SiLU()
        (2): Linear(in_features=128, out_features=1, bias=True)
      )
    )
  )
)>)]
property_stubs = []

    def create_methods_and_properties_from_stubs(
        concrete_type, method_stubs, property_stubs
    ):
        method_defs = [m.def_ for m in method_stubs]
        method_rcbs = [m.resolution_callback for m in method_stubs]
        method_defaults = [get_default_args(m.original_method) for m in method_stubs]
    
        property_defs = [p.def_ for p in property_stubs]
        property_rcbs = [p.resolution_callback for p in property_stubs]
    
>       concrete_type._create_methods_and_properties(
            property_defs, property_rcbs, method_defs, method_rcbs, method_defaults
        )
E       RuntimeError: 
E       Arguments for call are not valid.
E       The following variants are available:
E         
E         aten::mul.Tensor(Tensor self, Tensor other) -> Tensor:
E         Expected a value of type 'Tensor' for argument 'self' but instead found type 'int'.
E         
E         aten::mul.Scalar(Tensor self, Scalar other) -> Tensor:
E         Expected a value of type 'Tensor' for argument 'self' but instead found type 'int'.
E         
E         aten::mul.out(Tensor self, Tensor other, *, Tensor(a!) out) -> Tensor(a!):
E         Expected a value of type 'Tensor' for argument 'self' but instead found type 'int'.
E         
E         aten::mul.Scalar_out(Tensor self, Scalar other, *, Tensor(a!) out) -> Tensor(a!):
E         Expected a value of type 'Tensor' for argument 'self' but instead found type 'int'.
E         
E         aten::mul.left_t(t[] l, int n) -> t[]:
E         Could not match type int to List[t] in argument 'l': Cannot match List[t] to int.
E         
E         aten::mul.right_(int n, t[] l) -> t[]:
E         Could not match type Optional[Tensor] to List[t] in argument 'l': Cannot match List[t] to Optional[Tensor].
E         
E         aten::mul.int(int a, int b) -> int:
E         Expected a value of type 'int' for argument 'b' but instead found type 'Optional[Tensor]'.
E         
E         aten::mul.complex(complex a, complex b) -> complex:
E         Expected a value of type 'complex' for argument 'a' but instead found type 'int'.
E         
E         aten::mul.float(float a, float b) -> float:
E         Expected a value of type 'float' for argument 'a' but instead found type 'int'.
E         
E         aten::mul.int_complex(int a, complex b) -> complex:
E         Expected a value of type 'complex' for argument 'b' but instead found type 'Optional[Tensor]'.
E         
E         aten::mul.complex_int(complex a, int b) -> complex:
E         Expected a value of type 'complex' for argument 'a' but instead found type 'int'.
E         
E         aten::mul.float_complex(float a, complex b) -> complex:
E         Expected a value of type 'float' for argument 'a' but instead found type 'int'.
E         
E         aten::mul.complex_float(complex a, float b) -> complex:
E         Expected a value of type 'complex' for argument 'a' but instead found type 'int'.
E         
E         aten::mul.int_float(int a, float b) -> float:
E         Expected a value of type 'float' for argument 'b' but instead found type 'Optional[Tensor]'.
E         
E         aten::mul.float_int(float a, int b) -> float:
E         Expected a value of type 'float' for argument 'a' but instead found type 'int'.
E         
E         aten::mul(Scalar a, Scalar b) -> Scalar:
E         Expected a value of type 'number' for argument 'b' but instead found type 'Optional[Tensor]'.
E         
E         mul(float a, Tensor b) -> Tensor:
E         Expected a value of type 'float' for argument 'a' but instead found type 'int'.
E         
E         mul(int a, Tensor b) -> Tensor:
E         Expected a value of type 'Tensor' for argument 'b' but instead found type 'Optional[Tensor]'.
E         
E         mul(complex a, Tensor b) -> Tensor:
E         Expected a value of type 'complex' for argument 'a' but instead found type 'int'.
E       
E       The original call is:
E         File "/home/raul/work/bcn/torchmd-net/tests/test_model.py", line 74
E                   y, neg_dy = self.model(z, pos, batch=batch)
E                   # A TorchScript bug is triggered if we modify an output of model marked as Optional[Tensor]
E                   return y, 2*neg_dy
E                             ~~~~~~~~ <--- HERE

/home/raul/miniforge3/envs/torchmdnet/lib/python3.11/site-packages/torch/jit/_recursive.py:469: RuntimeError
```
</details>

This is due to a bug/limitation in TorchScript. One cannot script code in which an `Optional[Tensor]` is operated on. Since TorchMD_Net returns `Tuple[Tensor,Optional[Tensor]]` the error pops out.
In the test the error can be fixed if one convinces TorchScript that neg_dy is actually a Tensor:
```python
                y, neg_dy = self.model(z, pos, batch=batch)
                assert neg_dy is not None
                return y, 2*neg_dy
```
However, this requires an user to:
1. Know that TorchMD_Net returns `Tuple[Tensor,Optional[Tensor]]`. Which can be challenging to discover if the user is, for instance, loading some checkpoint.
2. Know about this limitation in TorchScript.

Otherwise the error is really unhelpful.

Another solution is to force TorchMD_Net to always return two Tensors. When `derivative` is False the second output is an empty tensor instead of None.
This PR implements this solution, making the test above pass.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0844242c6d50`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.