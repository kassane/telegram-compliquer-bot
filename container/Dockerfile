FROM python:3.8

ENV PYTHONUNBUFFERED True

# zig 0.7.1
WORKDIR /zig
RUN wget https://ziglang.org/download/0.7.1/zig-linux-x86_64-0.7.1.tar.xz
RUN tar xf zig-linux-x86_64-0.7.1.tar.xz
ENV PATH=/zig:$PATH

WORKDIR /app

COPY *.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY . .

RUN useradd -r user

USER user

CMD exec gunicorn --bind :$PORT --workers 1 --timeout 0 main:application
