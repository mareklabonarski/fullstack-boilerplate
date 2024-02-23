FROM node:20-alpine AS base

# Install dependencies only when needed
FROM base AS deps
# Check https://github.com/nodejs/docker-node/tree/b4117f9333da4138b03a546ec926ef50a31506c3#nodealpine to understand why libc6-compat might be needed.
RUN apk add --no-cache g++ make py3-pip libc6-compat
WORKDIR /app

# Install dependencies based on the preferred package manager
COPY package.json yarn.lock* package-lock.json* pnpm-lock.yaml* ./
RUN \
  if [ -f yarn.lock ]; then yarn --frozen-lockfile; \
  elif [ -f package-lock.json ]; then npm ci; \
  elif [ -f pnpm-lock.yaml ]; then corepack enable pnpm && pnpm i --frozen-lockfile; \
  else echo "Lockfile not found." && exit 1; \
  fi

#
#FROM base AS builder
#WORKDIR /nextjs
#COPY --from=base /nextjs/node_modules ./node_modules
#COPY . .
## RUN npm install
#RUN npm run build
#
#
#FROM builder as dev
#WORKDIR /nextjs
#COPY --from=builder /nextjs/public ./public
#COPY --from=builder /nextjs/.next ./.next
#COPY --from=builder /nextjs/node_modules ./node_modules
#COPY --from=builder /nextjs/package.json ./package.json
#COPY . .
#ENV NODE_ENV=development
#RUN npm install
#CMD ["npm", "run", "dev"]

FROM deps as dev

WORKDIR /frontend

COPY package.json ./

RUN npm install

COPY . .

CMD ["npm", "run", "dev"]