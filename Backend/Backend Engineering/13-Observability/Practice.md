# Practice — Observability

1. Add structured (JSON) logging to a small API you've built — log at least request received, request completed, and any errors, including a generated correlation/request ID on every log line for a given request.
2. Deliberately trigger an error in your API and confirm you can find the exact error log entry by searching for its correlation ID.
3. Install Prometheus + Grafana locally (or use a hosted free-tier option) and instrument a simple app to expose request count and latency metrics — build a basic dashboard showing them.
4. Set up one alert rule (e.g., "error rate > 10% over 5 minutes") and manually trigger it by generating errors in your test app.
5. If you have access to 2+ services communicating with each other, set up basic distributed tracing (Jaeger or OpenTelemetry) and generate a trace spanning both services — inspect the resulting trace visualization.
6. Explain in your own words the difference between logs, metrics, and traces, and give a realistic debugging scenario where each one specifically would be the most useful tool.

✅ Done? Move to `14-Advanced-Topics`.
