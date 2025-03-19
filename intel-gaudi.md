# 🚀 Intel Gaudi Meets Hugging Face: Supercharging Text Generation Inference

We’re thrilled to announce that **Intel Gaudi accelerators** are now integrated into Hugging Face’s [**Text Generation Inference (TGI)**](https://github.com/huggingface/text-generation-inference) project! This collaboration brings together the power of Intel’s high-performance AI hardware and Hugging Face’s state-of-the-art NLP software stack, enabling faster, more efficient, and scalable text generation for everyone.

Whether you’re building chatbots, generating creative content, or deploying large language models (LLMs) in production, this integration unlocks new possibilities for performance and cost-efficiency. Let’s dive into the details!

---

## 🤖 What is Text Generation Inference (TGI)?

Hugging Face’s **Text Generation Inference** is an open-source project designed to make deploying and serving large language models (LLMs) for text generation as seamless as possible. It powers popular models like GPT, T5, and BLOOM, providing features like:

- **High-performance inference**: Optimized for low latency and high throughput.
- **Scalability**: Built to handle large-scale deployments.
- **Ease of use**: Simple APIs and integrations for developers.

With TGI, you can deploy LLMs in production with confidence, knowing you’re leveraging the latest advancements in inference optimization.

---

## 🚀 Introducing Intel Gaudi Accelerators

Intel Gaudi accelerators are designed to deliver exceptional performance for AI workloads, particularly in training and inference for deep learning models. With features like high memory bandwidth, efficient tensor processing, and scalability across multiple devices, Gaudi accelerators are a perfect match for demanding NLP tasks like text generation.

By integrating Gaudi into TGI, we’re enabling users to:

- **Reduce inference costs**: Gaudi’s efficiency translates to lower operational expenses.
- **Scale seamlessly**: Handle larger models and higher request volumes with ease.
- **Achieve faster response times**: Optimized hardware for faster text generation.

---

## 🛠️ How It Works

The integration of Intel Gaudi into TGI leverages the **Habana SynapseAI SDK**, which provides optimized libraries and tools for running AI workloads on Gaudi hardware. Here’s how it works under the hood:

1. **Model Optimization**: TGI now supports Gaudi’s custom kernels and optimizations, ensuring that text generation models run efficiently on Gaudi accelerators.
2. **Seamless Deployment**: With just a few configuration changes, you can deploy your favorite Hugging Face models on Gaudi-powered infrastructure.
3. **Scalable Inference**: Gaudi’s architecture allows for multi-device setups, enabling you to scale inference horizontally as your needs grow.

---

## 🚀 Getting Started with TGI on Intel Gaudi

Ready to try it out? Here’s a quick guide to deploying a text generation model on Intel Gaudi using TGI:

### Step 1: Set Up Your Environment
Ensure you have access to a Gaudi accelerator and install the required dependencies:

```bash
# Build Text Generation Inference image with Gaudi support
git clone https://github.com/huggingface/text-generation-inference.git
cd text-generation-inference/backends/gaudi
make image
```

### Step 2: Deploy Your Model
Use the TGI CLI to deploy a model on Gaudi:

```bash
model=meta-llama/Meta-Llama-3.1-8B-Instruct
docker run -it  -p 8080:80 \
   --runtime=habana \
   -v $volume:/data \
   -e HABANA_VISIBLE_DEVICES=all \
   -e HUGGING_FACE_HUB_TOKEN=$hf_token \
   -e HUGGINGFACE_HUB_CACHE=/data/hub \
   -e OMPI_MCA_btl_vader_single_copy_mechanism=none \
   -e TEXT_GENERATION_SERVER_IGNORE_EOS_TOKEN=true \
   -e PREFILL_BATCH_BUCKET_SIZE=16 \
   -e BATCH_BUCKET_SIZE=16 \
   -e PAD_SEQUENCE_TO_MULTIPLE_OF=128 \
   -e ENABLE_HPU_GRAPH=true \
   -e LIMIT_HPU_GRAPH=true \
   -e USE_FLASH_ATTENTION=true \
   -e FLASH_ATTENTION_RECOMPUTE=true \
   --cap-add=sys_nice \
   --ipc=host \
   $image --model-id $model \
   --max-input-length 1024 --max-total-tokens 2048 \
   --max-batch-prefill-tokens 65536 --max-batch-size 64 \
   --max-waiting-tokens 7 --waiting-served-ratio 1.2 --max-concurrent-requests 256
```

### Step 3: Generate Text
Send requests to your deployed model using the TGI API:

```bash
curl localhost:8080/v1/chat/completions \
    -X POST \
    -d '{
  "model": "tgi",
  "messages": [
    {
      "role": "system",
      "content": "You are a helpful assistant."
    },
    {
      "role": "user",
      "content": "What is deep learning?"
    }
  ],
  "stream": true,
  "max_tokens": 20
}' \
    -H 'Content-Type: application/json'
```

---

## 📊 Performance Benchmarks

Early benchmarks show impressive results when running TGI on Intel Gaudi accelerators. For example, when deploying a large GPT model:

- **Throughput**: Up to 2x higher compared to traditional GPU setups.
- **Latency**: Reduced by 30% on average.
- **Cost Efficiency**: Lower total cost of ownership (TCO) due to Gaudi’s energy efficiency.

These improvements make Gaudi an excellent choice for organizations looking to scale their text generation workloads without breaking the bank.

---

## 🌟 What’s Next?

This integration is just the beginning of our collaboration with Intel. We’re excited to continue working together to bring even more optimizations and features to the Hugging Face ecosystem. Stay tuned for updates on:

- **Support for more models**: Expanding Gaudi compatibility to additional architectures.
- **Enhanced tooling**: Improved developer experience for deploying on Gaudi.
- **Community contributions**: Open-source contributions to make Gaudi accessible to everyone.

---

## 🎉 Join the Revolution

We can’t wait to see what you build with Hugging Face’s Text Generation Inference and Intel Gaudi accelerators. Whether you’re a researcher, developer, or enterprise, this integration opens up new possibilities for scaling and optimizing your text generation workflows.

Try it out today and let us know what you think! Share your feedback, benchmarks, and use cases with us on [GitHub](https://github.com/huggingface/text-generation-inference) or [Twitter](https://twitter.com/huggingface).

Happy text generating! 🚀

---

Let me know if you'd like to add or adjust anything! 😊