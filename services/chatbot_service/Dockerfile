FROM curlimages/curl as CERT

RUN curl -o /tmp/IntelSHA256RootCA.crt http://certificates.intel.com/repository/certificates/IntelSHA256RootCA-Base64.crt \
    && curl -o /tmp/IntelRootCA.crt http://certificates.intel.com/repository/certificates/Intel%20Root%20CA%20Base64.crt

FROM python:3.11-slim

WORKDIR /code

COPY . /code

RUN pip install --upgrade pip
RUN pip install --no-cache-dir --upgrade -r /code/requirements.txt


COPY --from=CERT /tmp/IntelSHA256RootCA.crt /usr/local/share/ca-certificates/
COPY --from=CERT /tmp/IntelRootCA.crt /usr/local/share/ca-certificates/
RUN update-ca-certificates

EXPOSE 80
ENTRYPOINT ["uvicorn", "main:api", "--host", "0.0.0.0", "--port", "80"]
CMD ["--workers", "2"]
