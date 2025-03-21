# Enhancing LLMs for Solving Competition-Level Algebra Problems Using RAG and CoT Prompting

This repository contains code and resources for the research project on evaluating and enhancing large language models (LLMs) for solving competition-level algebra problems. The project explores the effectiveness of Retrieval-Augmented Generation (RAG) and Chain-of-Thought (CoT) prompting techniques in improving the mathematical reasoning capabilities of LLMs.

- **Main Notebook**: `CSE256-FA24-Final-Project.ipynb` - Implements the experiments and evaluations for the research project.
- **Analysis Notebook**: `analysis-of-results/Analysis of Outcomes.ipynb` - Analyzes the results generated by the main notebook.
- **Results CSV**: `analysis-of-results/llm_results_comprehensive.csv` - Contains the results of the scenarios executed for each of the test cases run with DeepSeek-Prover and DeepSeek-Math models.
- **LaTeX Report**: `latex-report/` - Contains LaTeX files and the final project report.
- **PDF OCR Notebook**: `pdfs-ocr-nougat/convert-pdfs.ipynb` - Processes PDF papers for literature review summaries.

## Requirements

- Python 3.7 or higher
- PyTorch >= 2.4.0
- Transformers >= 4.45.1
- Datasets >= 3.0.1
- SymPy >= 1.13.3
- FAISS-GPU >= 1.7.2
- SentenceTransformers >= 2.7.0
- Ragatouille >= 0.0.8.post4
- Accelerate >= 0.34.2
- Matplotlib >= 3.7.5
- Google Generative AI Client >= 0.8.2

## Installation

1. **Clone the repository**:

   ```bash
   git clone https://github.com/ChestnutKurisu/CSE256_Final_Project_FA24.git
   cd CSE256_Final_Project_FA24
   ```

2. **Create a virtual environment**:

   ```bash
   python3 -m venv my-venv
   ```

3. **Activate the virtual environment**:

   - For **Linux** or **macOS**:

     ```bash
     source my-venv/bin/activate
     ```

   - For **Windows**:

     ```bash
     my-venv\Scripts\activate
     ```

4. **Install the required packages**:

   ```bash
   pip install -r requirements.txt
   ```

   **Note**: Ensure that you have CUDA installed and configured if you plan to run the models on a GPU.

## Usage

### Running Experiments

Due to the computational resources required to run the experiments, it is recommended to use the provided Kaggle notebook:

- **Kaggle Notebook**: [CSE 256 FA24 Final Project](https://www.kaggle.com/code/amadeuskurisu/cse-256-fa24-final-project)

You can **fork** the notebook and run it directly on Kaggle's platform, which provides free GPU resources.

#### Steps to Run on Kaggle

1. **Sign in to Kaggle**: If you don't have an account, you'll need to create one.

2. **Access the Notebook**:

   - Visit the [Kaggle notebook link](https://www.kaggle.com/code/amadeuskurisu/cse-256-fa24-final-project).
   - Click on the **"Copy and Edit"** button to fork the notebook into your own account.

3. **Set Up the Environment**:

   - Ensure that the notebook is set to use a GPU. Go to **Settings** and select **GPU** under the **Accelerator** option.

4. **Run the Notebook**:

   - Execute the cells sequentially to run the experiments.

### Running Locally

If you prefer to run the code locally, follow these steps:

1. **Download the Required Models**:

   - **DeepSeek-Prover-V1.5-RL**: [Download Link](https://huggingface.co/deepseek-ai/DeepSeek-Prover-V1.5-RL)
   - **DeepSeek-Math-7B-RL**: [Download Link](https://huggingface.co/deepseek-ai/deepseek-math-7b-rl)
   - **ColBERTv2.0**: [Download Link](https://huggingface.co/colbert-ir/colbertv2.0)

   Place the models in the appropriate directories or update the paths in the notebook to point to the downloaded models.

2. **Run the Jupyter Notebook**:

   ```bash
   jupyter notebook kaggle/CSE256-FA24-Final-Project.ipynb
   ```

   **Note**: Ensure you have access to a GPU with sufficient memory (at least 16 GB VRAM) to load and run the models.

3. **Configure API Keys**:

   - The notebook uses the Google Generative AI API for embedding generation.
   - Obtain API keys and update the `GOOGLE_AI_API_KEYS` list in the notebook with your keys.

4. **Run the Cells**:

   - Execute the cells in the notebook sequentially.
   - Be mindful of the computational constraints and adjust parameters like `NUM_RETRIEVED_DOCS` and `num_samples` if necessary.

### Analyzing Results

- Open the analysis notebook:

  ```bash
  jupyter notebook analysis-of-results/Analysis\ of\ Outcomes.ipynb
  ```

- This notebook processes the results and generates visualizations and performance metrics.

## Results Summary

The project evaluates different methods and models on a subset of challenging algebra problems from the MATH benchmark. Key findings include:

- **DeepSeek-Prover-V1.5-RL** significantly outperforms **DeepSeek-Math-7B-RL** on competition-level algebra problems.
- Combining **Chain-of-Thought prompting** with advanced models improves accuracy.
- **Retrieval-Augmented Generation (RAG)** showed limited improvement due to computational constraints and the size of the retrieval corpus.
- **Self-Consistency Decoding** enhances performance up to a point, but increasing the number of samples beyond three did not yield further improvements under limited computational resources.

For detailed results and analysis, refer to the notebooks and the final report in the `latex-report/` directory.

## Data

- **Dataset**: A subset of 50 hard (difficulty level 5) algebra problems from the [MATH dataset](https://huggingface.co/datasets/ontocord/lighteval_MATH-Hard).
- **Models Used**:
  - [DeepSeek-Prover-V1.5-RL](https://huggingface.co/deepseek-ai/DeepSeek-Prover-V1.5-RL)
  - [DeepSeek-Math-7B-RL](https://huggingface.co/deepseek-ai/deepseek-math-7b-rl)
  - [ColBERTv2.0](https://huggingface.co/colbert-ir/colbertv2.0)

## Notes

- **Computational Resources**: The experiments require significant computational resources, especially GPU memory. Ensure your environment meets the requirements.
- **Kaggle Environment**: The provided Kaggle notebook is optimized to run within Kaggle's free GPU environment, which offers limited computational power.
- **API Usage**: The code uses external APIs (e.g., Google Generative AI API). Be aware of any usage limits or costs associated with these services.
- **Reproducibility**: Set seeds and parameters are configured for reproducibility, but results may vary slightly due to the stochastic nature of model inference.

## Contact

For any questions or issues, please contact [psomane@ucsd.edu](mailto:psomane@ucsd.edu).
