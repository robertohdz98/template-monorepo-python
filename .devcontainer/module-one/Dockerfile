FROM python:3.10-slim-bullseye

RUN apt-get update --fix-missing
RUN apt-get install -y git make curl nano vim wget

ENV HOMEDIR=/app
RUN mkdir -p $HOMEDIR

WORKDIR $HOMEDIR
ENV PYTHONPATH=$PYTHONPATH:/app

# Non-root user
ARG USERNAME=user
ARG USER_UID=1000
ARG USER_GID=$USER_UID
RUN groupadd --gid $USER_GID $USERNAME \
  && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME

RUN pip install --no-cache-dir --upgrade pip
RUN pip install poetry

COPY poetry.lock poetry.lock
COPY pyproject.toml pyproject.toml
COPY . $HOMEDIR

RUN poetry config virtualenvs.create false && poetry install

USER $USERNAME