FROM python:3.8

ENV PYTHONUNBUFFERED True

WORKDIR /app

COPY *.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY . .

RUN useradd -r user

USER user

ENV USER=user
ENV PATH=/user/zig:$PATH

SHELL ["bash", "-c"]

RUN \
    apt update && apt install jq curl wget -y && \
    curl -s https://ziglang.org/download/index.json \
    | jq --raw-output '.master."x86_64-linux".tarball' \
    | wget -q --show-progress -i -; \
    file=$(ls | grep 'zig*.*.*') && \
    tar -xvf $file &&\
    folder=$file \
    folder=${folder%.*.*} \
    && mkdir -p /user/zig &&\
    mv -v $folder/* /user/zig &&\
    rm -rvf $folder && echo "Zig version: $(zig version)"

CMD exec gunicorn --bind :$PORT --workers 1 --timeout 0 main:application
