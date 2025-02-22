Provide your CLI command here:
grep '"symbol": "TSLA"' transaction-log.txt | grep '"side": "sell"' | awk -F'"' '{print $5}' | xargs -I {} curl -s "https://example.com/api/{}" >> output.txt