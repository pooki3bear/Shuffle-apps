# Base our app image off of the WALKOFF App SDK image
FROM frikky/shuffle:app_sdk as base

# We're going to stage away all of the bloat from the build tools so lets create a builder stage
# Install all alpine build tools needed for our pip installs
FROM base as builder
RUN apk --no-cache add --update alpine-sdk libffi libffi-dev musl-dev openssl-dev

# Install all of our pip packages in a single directory that we can copy to our base image later
RUN mkdir /install
WORKDIR /install
#COPY requirements.txt /requirements.txt
#RUN pip install --prefix="/install" -r /requirements.txt
#RUN pip3 install tika==1.24
#RUN pip3 install requests==2.25.1
#RUN pip3 install spacy==2.3.5
#RUN pip3 install cyberspacy==1.1.1
RUN pip3 install tika
RUN pip3 install requests
RUN pip3 install spacy
RUN pip3 install cyberspacy

# Switch back to our base image and copy in all of our built packages and source code
FROM base
COPY --from=builder /install /usr/local
COPY src /app

# Install any binary dependencies needed in our final image
# RUN apk --no-cache add --update my_binary_dependency
RUN apk add --no-cache libstdc++ openjdk8
RUN python3 -m spacy download en_core_web_lg
RUN python3 -c 'import tika; from tika import parser;tika.initVM(); parser.from_file("/bin/sh")'; echo "tika prepared"
# Finally, lets run our app!
WORKDIR /app
CMD python3 app.py --log-level DEBUG
