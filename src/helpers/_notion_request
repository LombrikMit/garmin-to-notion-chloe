import time

from notion_client.errors import APIResponseError


def notion_request_with_retry(request, **kwargs):
    """
    Execute a Notion request with rate limiting and automatic retries.
    """
    max_attempts = 6

    for attempt in range(max_attempts):
        time.sleep(0.4)

        try:
            return request(**kwargs)

        except APIResponseError as error:
            if getattr(error, "status", None) != 429:
                raise

            if attempt == max_attempts - 1:
                raise

            wait_time = 2 * (attempt + 1)
            print(f"Notion rate limit: waiting {wait_time}s before retry.")
            time.sleep(wait_time)
