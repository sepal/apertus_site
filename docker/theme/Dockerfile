FROM node:6

# Install yarn
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - \
  && echo "deb http://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list \
  && apt-get update && apt-get install -y yarn

# Install gulp
RUN yarn global add gulp

# Copy the sources
COPY ./web/themes/custom/apertus /theme

WORKDIR /theme

# Install dependencies
RUN yarn install

CMD ['gulp']
