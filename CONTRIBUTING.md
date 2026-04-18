# Contributing to MLX LM 

We want to make contributing to this project as easy and transparent as
possible.

## Pull Requests

1. Fork and submit pull requests to the repo. 
2. If you've added code that should be tested, add tests.
3. Every PR should have passing tests and at least one review. 
4. For local development, sync the repo with `uv` and install the pre-commit hooks:
   ```bash
   uv sync
   uv run pre-commit install
   ```
   This will install the project in editable mode along with the Python tooling
   used for formatting and local checks.
 
   You can also run the formatters manually as follows on individual files:
 
     ```bash
     clang-format -i file.cpp
     ```
 
     ```bash
     uv run black file.py
     uv run isort --profile=black file.py
     ```

     or,

     ```bash
     # single file
     uv run pre-commit run --files file1.py 

     # specific files
     uv run pre-commit run --files file1.py file2.py
     ```
 
   or run `uv run pre-commit run --all-files` to check all files in the repo.

## Issues

We use GitHub issues to track public bugs. Please ensure your description is
clear and has sufficient instructions to be able to reproduce the issue.

## License

By contributing to mlx-lm, you agree that your contributions will be licensed
under the LICENSE file in the root directory of this source tree.

## Adding New Models

Below are some tips to port LLMs available on Hugging Face to MLX.

From this directory, do an editable install:

```shell
uv sync
```

Then check if the model has weights in the
[safetensors](https://huggingface.co/docs/safetensors/index) format. If not
[follow instructions](https://huggingface.co/spaces/safetensors/convert) to
convert it.

After that, add the model file to the
[`mlx_lm/models`](https://github.com/ml-explore/mlx-lm/tree/main/mlx_lm/models)
directory. You can see other examples there. We recommend starting from a model
that is similar to the model you are porting.

Make sure the name of the new model file is the same as the `model_type` in the
`config.json`, for example
[starcoder2](https://huggingface.co/bigcode/starcoder2-7b/blob/main/config.json#L17).

To determine the model layer names, we suggest either:

- Refer to the Transformers implementation if you are familiar with the
  codebase.
- Load the model weights and check the weight names which will tell you about
  the model structure.
- Look at the names of the weights by inspecting `model.safetensors.index.json`
  in the Hugging Face repo.

To add LoRA support edit
[`mlx_lm/tuner/utils.py`](https://github.com/ml-explore/mlx-lm/blob/main/mlx_lm/tuner/utils.py#L27-L60)

Finally, add a test for the new modle type to the [model
tests](https://github.com/ml-explore/mlx-lm/blob/main/tests/test_models.py).

You can run the tests with:

```shell
uv run python -m unittest discover tests/
```
