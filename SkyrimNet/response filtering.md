I'm begging for the next release
```
std::string filter_response(const std::string& resp) {
    std::regex re_response("┤([^├]+)├");
    std::smatch m;
    if (std::regex_search(resp, m, re_response)) {
        std::string resp_filtered = m[1];
        if (resp_filtered.length() + 2 != resp.length()) {
            log("LLM response included " +
                std::to_string(resp.length() - 2 - resp_filtered.length()) +
                " additional characters, this will increase latency, consider an alternative model");
        }
        return resp_filtered;
    }
    log("LLM response was empty");
    return "";
}
```

