# Donut: OCR-free Document Understanding Transformer

## Background/Motivation

Optical Character Recognition (OCR) is a technology used to convert different types of documents, such as scanned paper documents, PDF files, or images captured by a digital camera, into editable and searchable data. An example of its application is in the use of Rocketbook for handwritten class notes, which are digitized through OCR. Beyond academic notes, OCR is crucial in business for automating the processing of documents like invoices and receipts, enhancing data entry, archiving, and content management.

### Common Tasks in Visual Document Understanding (VDU)
- Document classification
- Information extraction
- Visual question answering

## Problems with Traditional OCR for VDU

The traditional OCR process for VDU is a two-stage process:

1. **Stage 1:** Capturing text using OCR.
2. **Stage 2:** Modeling the document's holistic understanding.

![image](https://github.com/jiax264/DS5690_Donut_Paper_Presentation/assets/64748973/1a50a614-17dd-49a4-9df3-f5917826c400)

### Issues with OCR in VDU:
- **High Cost:** OCR as pre-processing is computationally expensive.
- **Inflexibility:** Struggles with language and document-type variations, leading to poor generalization ability.
- **Error Propagation:** OCR mistakes affect the entire VDU process, especially with complex characters like Korean or Chinese.

## How Donut Addresses these Problems
- **OCR-Free Mapping:** Directly translates raw images into outputs, bypassing traditional OCR.
- **Transformer Architecture:** Utilizes end-to-end Transformer-based models for reading and understanding documents.
- **Pre-training and Fine-Tuning:** Employs a two-step training process involving pre-training on images and annotations and fine-tuning on specific tasks.
- **Language and Domain Flexibility:** Achieves versatility across languages and domains using synthetic data during pre-training.
- **Performance:** Demonstrates superior speed and accuracy in VDU tasks across various benchmarks and datasets.

### Donut’s Approach
![image](https://github.com/jiax264/DS5690_Donut_Paper_Presentation/assets/64748973/aa8f91ff-c6ac-4f07-8bb8-8f572e90cae1)

Donut uses a visual encoder to interpret the document image and a textual decoder to produce a JSON structured format without relying on OCR. Each model component is Transformer-based, and thus the model is trained easily in an end-to-end manner

### Gradio Demos
- Document Classification: [Donut-RVLCDIP](https://huggingface.co/spaces/nielsr/donut-rvlcdip)
- Document Information Extraction: [Donut-Base Fine-tuned CORD v2](https://huggingface.co/spaces/naver-clova-ix/donut-base-finetuned-cord-v2)
- Document Visual Question Answering: [Donut-DoCvQA](https://huggingface.co/spaces/nielsr/donut-docvqa)

## Brief Critical Analysis on Results

![image](https://github.com/jiax264/DS5690_Donut_Paper_Presentation/assets/64748973/0f6692ce-be1a-48c6-b383-c9264b01e220)

While Donut demonstrates the fastest inference time and superior performance across all tested datasets, the lack of error analysis in the paper is a notable omission for further advancements and reliability of the model.

## Donut Pseudocode
![image](https://github.com/jiax264/DS5690_Donut_Paper_Presentation/assets/64748973/e1993360-8893-4b02-aa92-452877c040f7)

### Question 1: What is the input to the encoder?
![image](https://github.com/jiax264/DS5690_Donut_Paper_Presentation/assets/64748973/430c70fb-9bf9-4d45-9c2e-21697a3d9c5f)

The process includes flattening 2D patches into 1D arrays and encoding their positions within the image. This allows the Transformer to focus on important patches using the attention mechanism.

### Question 2: Why use patches and why not flatten the entire image into one long 1D array? 
<details>
  <summary>Hint 1</summary>
  Consider how the number of comparisons in a Transformer's attention mechanism changes with the length of the input sequence.
</details>

<details>
  <summary>Hint 2</summary>
  Imagine moving an object in an image by a few pixels. How might this affect the input if it’s represented pixel by pixel versus in patches?
</details>

<details>
  <summary>Answer</summary>
  Answer: By using patches, a model retains local structure, directly capturing features like the top of a building within a cohesive region. Processing pixel by pixel can be highly sensitive to shifts, causing translation variance where small changes lead to disproportionate impacts on the model's output. This pixel-based approach is also computationally intensive. Patches, on the other hand, enable parallel processing and result in a more efficient attention mechanism due to fewer parameters and a reduced need for pairwise comparisons.
</details>

![image](https://github.com/jiax264/DS5690_Donut_Paper_Presentation/assets/64748973/4c78a32e-9e11-4f33-ab39-4acde25fbfcb)

### Question 3: In line 6, we encounter the multi-head attention mechanism, where 'Y' denotes the sequence of tokens generated up to this point, and we incorporate the multi-head attention weights. Now, considering the integration of visual information, can anyone clarify the role of 'z' in this context?"
<details>
  <summary>Answer</summary>
Answer: The term 'z' represents a collection of embeddings produced by the encoder. Each embedding within 'z' is aligned with distinct segments of the input image, effectively capturing and condensing both the visual attributes and the spatial layout into a structured format that the Transformer's decoder can interpret and utilize.
</details>

## Resource Links

- [Donut Code Repository](https://github.com/clovaai/donut/tree/master)
- [How does OCR work and what are some use cases](https://www.ibm.com/blog/optical-character-recognition/)
- [More about Image Patching](https://arxiv.org/pdf/2010.11929.pdf)
- [More about Donut’s Encoder: Swin Transformer](https://arxiv.org/pdf/2103.14030.pdf)
- [More about Donut’s Decoder: BART](https://arxiv.org/pdf/1910.13461.pdf)

## Paper Citation

G. Kim, T. Hong, M. Yim, J. Nam, J. Yim, W. Hwang, S. Yun, D. Han, S. Park, "OCR-free Document Understanding Transformer," in arXiv:2111.15664 [cs.LG], Nov. 2021, Accessed on: Mar. 6, 2024. [Online]. Available: [arXiv:2111.15664](https://arxiv.org/pdf/2111.15664.pdf)
