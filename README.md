
<a name="readme-top"></a>

<!-- PROJECT LOGO -->
<br />
<div align="center">
  <h3 align="center">Model Manager v0.1, by</h3>

  ![Color logo - no background](https://github.com/openfoundry-ai/model_manager/assets/152243036/75d1b0d1-1ca3-4260-afd1-4130b0c3db32)

  <p align="center">
    Deploy open source AI models to AWS in minutes.
    <br />
  </p>
</div>



<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about-model-manager">About Model Manager</a>
    </li>
    <li>
      <a href="#getting-started">Getting Started</a>
      <ul>
        <li><a href="#prerequisites">Prerequisites</a></li>
        <li><a href="#installation">Installation</a></li>
      </ul>
    </li>
    <li><a href="#using-model-manager">Using Model Manager</a></li>
    <li><a href="#what-were-working-on-next">What we're working on next</a></li>
    <li><a href="#known-issues">Known issues</a></li>
    <li><a href="#contributing">Contributing</a></li>
    <li><a href="#license">License</a></li>
    <li><a href="#contact">Contact</a></li>
  </ol>
</details>

<!-- ABOUT THE PROJECT -->
## About Model Manager
Model Manager is a Python tool that simplifies the process of deploying an open source AI model to your own cloud. Instead of spending hours digging through documentation to figure out how to get AWS working, Model Manager lets you deploy open source AI models directly from the command line.

Choose a model from Hugging Face or SageMaker, and Model Manager will spin up a SageMaker instance with a ready-to-query endpoint in minutes.

Here we’re deploying Microsoft’s Phi2. Larger models such as this take about 10 minutes to spin up.

https://github.com/openfoundry-ai/model_manager/assets/164248540/f7fbf9ce-04c3-49b0-b5be-2977b5cc90af

<br>
Once the model is running, you can query it to get a response.

![Screenshot 2024-03-20 at 6 01 44 PM](https://github.com/openfoundry-ai/model_manager/assets/164248540/20b46f8f-da01-4cc7-8343-e647b27ba7c6)


<p align="right">(<a href="#readme-top">back to top</a>)</p>


<!-- GETTING STARTED -->
<br>

## Getting Started

Model Manager works with AWS. Azure and GCP support are coming soon!
To get a local copy up and running follow these simple steps.

### Prerequisites

* Python
* An AWS account
* Quota for AWS SageMaker instances (by default, you get 2 instances of ml.m5.xlarge for free)
* Certain Hugging Face models (e.g. Llama2) require an access token ([hf docs](https://huggingface.co/docs/hub/en/models-gated#access-gated-models-as-a-user))


### Installation

**Step 1: Set up AWS and SageMaker**

To get started, you’ll need an AWS account which you can create at https://aws.amazon.com/. Then you’ll need to create access keys for SageMaker.

We made a walkthrough video to show you how to get set up with your SageMaker access keys in 2 minutes.

https://github.com/openfoundry-ai/model_manager/assets/164248540/52b0dcee-87cd-48de-9251-b2d3571abf61

If you prefer a written doc, we wrote up the steps in [Google Doc](https://docs.google.com/document/d/1kLzPU43kvLAoYzmBfvAkINmXZ24JzAp_fdBIYjJRXvU/edit?usp=sharing) as well. 


**Step 2: Set up Model Manager**

You should now have your Access Key and Secret from SageMaker. Now you can set up Model Manager! Clone the repo to your local machine, and then run the setup script in the repo:

```sh
   bash setup.sh
```

This will configure the AWS client so you’re ready to start deploying models. You’ll be prompted to enter your Access Key and Secret here. You can also specify your AWS region. The default is us-east-1. You only need to change this if your SageMaker instance quota is in a different region.

Optional: If you have a Hugging Face Hub Token, you can add it to `.env` that was generated by the setup script and add it with the key:

```sh
HUGGING_FACE_HUB_KEY="KeyValueHere"
```

This will allow you to use models with access restrictions such as Llama2 as long as your Hugging Face account has permission to do so.


<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- USAGE -->
<br>

## Using Model Manager
After you’ve set up AWS and Model Manager per the above, run Model Manager using python or python3:
```sh
python3 model_manager.py
```

![home screen](https://github.com/openfoundry-ai/model_manager/assets/164248540/22ede5aa-99e3-4191-bca8-3021c6b572e7)


Now you’re ready to start shipping models onto your cloud!
<br>
<br>

### Deploying models

There are two repositories from where you can deploy models: Hugging Face or SageMaker. Use whichever works for you! If you're deploying with Hugging Face, copy/paste the full model name from Hugging Face. For example, "google-bert/bert-base-uncased" without quotes. Note that you’ll need larger, more expensive instance types in order to run bigger models. It takes anywhere from 2 minutes (for smaller models) to 10+ minutes (for large models) to spin up the instance with your model.
<br>
<br>

If you’re using the ml.m5.xlarge instance type, here are some small Hugging Face models that work great:
<br>
<br>

**Model: [google-bert/bert-base-uncased](https://huggingface.co/google-bert/bert-base-uncased)**

- **Type:** Fill Mask: tries to complete your sentence like Madlibs
- **Query format:** text string with `[MASK]` somewhere in it that you wish for the transformer to fill
    
    ![fill mask bert query](https://github.com/openfoundry-ai/model_manager/assets/164248540/a8a6d8e9-183f-4c85-afe8-b9c21d8aa687)

<br>
<br>

**Model: [sentence-transformers/all-MiniLM-L6-v2](https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2)**

- **Type:** Feature extraction: turns text into a 384d vector embedding for semantic search / clustering
- **Query format:** "*type out a sentence like this one.*"
    
    ![sentence transformer query](https://github.com/openfoundry-ai/model_manager/assets/164248540/57b4da43-03bb-4642-892b-5f287dfce0d8)

<br>
<br>

**Model: [deepset/roberta-base-squad2](https://huggingface.co/deepset/roberta-base-squad2)**

- **Type:** Question answering; provide a question and some context from which the transformer will answer the question.
- **Query format:** A dict with two keys: `question` and `context`. For our tool, we will prompt you a second time to provide the context.

    ![roberta eqa query](https://github.com/openfoundry-ai/model_manager/assets/164248540/2054fdb7-1f3d-4bfe-a806-c5ebde8ad20d)

<br>
<br>

### Querying models
There are two ways to query a model you’ve deployed: you can query it using the Model Manager script, or you can call it directly from your code using SageMaker’s API.

Querying within Model Manager currently works for text-based models. Image generation, multi-modal, etc. models are not yet supported.

You can query all deployed models using the SageMaker API. Documentation for how to do this can be found [here](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_runtime_InvokeEndpoint.html).

### Deactivating models

Any model endpoints you spin up will run continuously unless you deactivate them! Make sure to delete endpoints you’re no longer using so you don’t keep getting charged for your SageMaker instance.


<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- ROADMAP -->
<br>

## What we're working on next
- [ ]  More robust error handling for various edge cases
- [ ]  Verbose logging
- [ ]  Support for deploying custom models
- [ ]  Single-line-of-code SageMaker onboarding
- [ ]  Enabling / disabling autoscaling
- [ ]  Deployment to Azure and GCP

<p align="right">(<a href="#readme-top">back to top</a>)</p>


<!-- ISSUES -->
<br>

## Known issues
- [ ]  Querying within Model Manager currently only works with text-based model - doesn’t work with multimodal, image generation, etc.
- [ ]  Long HF model names (longer than 50 characters) and names with special characters other  than `-` breaks.
- [ ]  Model versions are static.
- [ ]  Querying endpoints with different expected formats
- [ ]  Deleting a model is not instant, it may show up briefly after it was queued for deletion
- [ ]  Deploying the same model within the same minute will break

See [open issues](https://github.com/UnidataHQ/model_manager/issues) for a full list of known issues and proposed features.

<p align="right">(<a href="#readme-top">back to top</a>)</p>


<!-- CONTRIBUTING -->
<br>

## Contributing

If you have a suggestion that would make this better, please fork the repo and create a pull request. You can also simply open an issue with the tag "enhancement".

If you found this useful, please give us a star! Thanks again!

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- LICENSE -->
<br>

## License

Distributed under the MIT License. See `LICENSE.txt` for more information.

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- CONTACT -->
<br>

## Contact

You can reach us, Arthur & Tyler, at [hello@openfoundry.ai](mailto:hello@openfoundry.ai). 

We’d love to hear from you! We’re excited to learn how we can make this more valuable for the community and welcome any and all feedback and suggestions.
