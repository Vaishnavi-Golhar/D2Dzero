FROM golang:1.22.2-alpine

WORKDIR /app

#  Add curl
RUN apk add --no-cache curl

# Copy go.mod before source
COPY go.mod ./
RUN go mod tidy

# Copy rest and build
COPY . .
RUN go build -o main .

EXPOSE 8001

CMD ["./main"]

