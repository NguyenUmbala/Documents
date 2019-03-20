# Docker

## Giới thiệu về Docker:

Docker là một công cụ hỗ trợ việc tạo môi trường ảo (container) nhanh, gọn, đơn giản.

Đặc điểm của Docker là nhanh, nhẹ do Docker tạo các OS ảo. Các container sử dụng chung các images nên không tốn nhiều disk.

Docker images là các files và settings sử dụng trong container được lưu lại và sử dụng lại.

Docker Hub là trang web chia sẻ và lưu trữ các images.

Dockerfile là file chứa các settings và được định nghĩa rõ ràng bằng code. Giúp tự động hóa các thao tác và tránh gặp lỗi trong quá trình sử dụng.

Docker container thực hiện các process trên môi trường OS ảo (môi trường OS ảo được tạo ra từ Docker images). Điều đó giúp container chạy nhanh hơn, nhẹ hơn, đỡ tốn tài nguyên so với máy ảo thông thường. 

## Install and Docker:

- Link hướng dẫn (Step 1, 2): https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-18-04

## Các bước build docker container cho golang:

- Bước 1: Viết code cho app:

- Bước 2: Tạo Dockerfile:
    - Example:
        ```
        # Start from golang v1.12 base image
        FROM golang:1.12

        # Set current working directory
        WORKDIR $GOPATH/src/myapp

        # Copy everything in current working directory
        COPY . .

        # Download all dependencies
        RUN go get github.com/gin-gonic/gin
        RUN go get github.com/adlio/trello
        RUN go get github.com/jinzhu/gorm
        RUN go get github.com/mattn/go-sqlite3
        RUN go get github.com/stretchr/testify/assert

        # Build my app    
        RUN go build -o myapp

        # This container exposes port 8080 to the outside world
        EXPOSE 8080

        # Run the executable
        CMD [ "./myapp" ]
        ```

- Bước 3: Build the image:
    - Example:
        ```
        $ docker build -t myapp .
        ```

- Bước 4: Run the image:
    - Example:
        ```
        $ sudo docker run --publish 6060:8080 --name test --rm myapp
        ```

## Một số câu lệnh Docker đơn giản:

- Image:
    ```
    - Build image:
    $ docker build -t <app_name>

    - Danh sách images:
    $ docker images

    - Pull image về máy tính:
    $ docker pull <image_name : version> 

    - Xóa 1 image:
    $ docker rmi <id | name> 

    - Xóa toàn bộ image:
    $ docker rmi $(docker images -q)
    ```

- Container:
    ```
    - Create and run container:
    $ docker run --publish <host_port:guest_port> --name <container_name> --rm <image_name>

    - Danh sách containers đang chạy:
    $ docker ps

    - Danh sách tất cả container:
    # docker ps -a

    - Khởi động container:
    $ docker start <id | name>

    - Dừng container đang chạy:
    $ docker stop <id | name>

    - Dừng tất cả containẻr đang chạy:
    $ docker stop $(docker ps -a -q)

    - Xóa 1 container:
    $ docker rm <id | name>

    - Xóa toàn bộ container:
    $ docker rm $(docker ps -a -q)

    - Truy cập vào 1 container:
    $ docker exec -it <id | name > bash

    - Kiểm tra log của 1 container:
    $ docker log <id | name> bash
    ```