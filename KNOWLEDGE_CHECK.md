# Lab 09: Knowledge check

**Answer each question below it, in your own words, in one to three sentences.** Replace the `...` below **Your answer:** with your text.

## 1. Credentials in the image?

Assume you want to share your image with others and make it publicly available on `ghcr.io`, so that anyone can pull it and run the analysis. A teammate suggests adding `COPY .aws /home/app/.aws` to the `Dockerfile`, so that nobody has to type `-v ~/.aws:/home/app/.aws:ro` any more. (This is a thought experiment: keep your own package private.)

**Your answer:** Give two reasons why this is a bad idea. Think of who could then read your credentials, and of what happens when your Learner Lab session ends.

The teammate's suggestion of copying AWS credentials into the image is a bad idea, because this means anyone who wants to download the image is able to access them. Also, every Learner Lab session expires, so the credentials would as well, and the image would have outdated credentials. 

## 2. Where did the Atlas password come from?

In Step 4, you ran the analysis once with `-v ~/.aws:/home/app/.aws:ro` only, without the three `-e` options. The container still wrote to your Atlas cluster.

*Hint:* Read `get_atlas_settings()` in `src/book_analysis/analyze.py`.

**Your answer:** Explain, step by step, where the container found the Atlas settings in that run.

The container used the AWS credentials from my computer to access the AWS, and because the environment variables were not provided, get_atlas_settings() got the Atlas settings from the SSM parameter and the password. 

## 3. Same tag, different image

Run `docker images book-analysis`, and note the image ID of `book-analysis:0.1.0`. Then change the `description` in the `LABEL` of your `Dockerfile`, build again with the same command, `docker build -t book-analysis:0.1.0 .`, and run `docker images book-analysis` again.

**Your answer:** Paste the image ID from before and after the rebuild. Why is it a problem for someone who uses your image that `0.1.0` now names a different image, and what should you have done instead?

Before the rebuild: 4da41676499b
After the rebuild: 4662e5ab8c16

This is a problem for someone who uses my image that 0.1.0 now names a different image, because they might get a different image than what was originally used, even though the tag is the same. Instead, I should have created a new version tag, rather than reusing the same one. 
