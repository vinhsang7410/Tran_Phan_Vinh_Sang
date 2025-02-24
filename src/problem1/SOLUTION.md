Provide your CLI command here:
grep '"symbol": "TSLA"' transaction-log.txt | grep '"side": "sell"' | awk -F'"' '{print $5}' | xargs -I {} curl -s "https://example.com/api/{}" >> output.txt
jq -r 'select(.symbol == "TSLA" and .side == "sell") | .order_id' transaction-log.txt | xargs -I {} curl "https://example.com/api/{}" >> output.txt