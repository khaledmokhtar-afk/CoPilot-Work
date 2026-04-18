# builder
FROM node:18-alpine AS builder
WORKDIR /work
COPY package.json pnpm-lock.yaml ./
RUN npm i -g pnpm
COPY . .
WORKDIR /work/apps/hub
RUN pnpm install --frozen-lockfile
RUN pnpm build

# runner
FROM node:18-alpine AS runner
WORKDIR /app
ENV NODE_ENV=production PORT=8080
RUN npm i -g pnpm
COPY --from=builder /work/apps/hub/package.json ./package.json
COPY --from=builder /work/apps/hub/.next ./.next
COPY --from=builder /work/apps/hub/public ./public
RUN pnpm install --prod
EXPOSE 8080
CMD ["pnpm","start"]
