# GITLAB-CI/CD

## Giới thiệu về CI/CD

CI/CD là bộ đôi công việc bao gồm Continuous Integration (tích hợp thường xuyên) và Continuous Delivery (cập nhật thường xuyên). Có nghĩa là tích hợp thường xuyên, nhanh chóng khi code cũng như thường xuyên cập nhật phiên bản mới.

## Cách sử dụng gitlab-ci/cd

- Bước 1: Cài đặt gitlab:
    ```
    $ docker run -d --name local-gitlab --restart always -p 80:80 gitlab/gitlab-ce
    ```

- Bước 2: Cài đặt gitlab-runner
    ```
    $ docker run -d --name gitlab-runner --restart always \
        -v /srv/gitlab-runner/config:/etc/gitlab-runner \
        -v /var/run/docker.sock:/var/run/docker.sock \
        gitlab/gitlab-runner:latest
    ```
    - Link hướng dẫn chi tiết: https://techmaster.vn/posts/34683/setup-he-thong-ci-voi-gitlab

- Bước 3: Đăng kí cho gitlap-runner
    ```
    $ sudo gitlab-runner register -n \
        --url http://localhost/ \
        --registration-token urCaUTrmzs2c_-JDcvU- \
        --executor docker \
        --description "test" \
        --docker-image "docker:stable" \
        --docker-volumes /var/run/docker.sock:/var/run/docker.sock

    $ sudo gitlab-runner restart
    ```

- Bước 4: Viết file .gitlab-ci.yml
    - Các thuộc tính cần thiết:
        - image: là image để run app
        - stage: là trạng thái như build, test, compile,...
        - before_script: các script chạy trước để cài đặt môi trường
        - script: là các script chạy chính tùy loại stage
        - ...

    - Example:
        ```
        image: golang

        Test:
            stage: test

            before_script:
                - cd $GOPATH/src
                - git clone https://gitlab.com/NguyenUmbala/trello-report-tools/
                - go get github.com/gin-gonic/gin
                - go get github.com/adlio/trello
                - go get github.com/stretchr/testify/assert
                - go get github.com/jinzhu/gorm
                - go get github.com/mattn/go-sqlite3

            script:
                - cd $GOPATH/src/trello-report-tools
                - go test ./...
        ```

    - Lưu ý:
        - Đầu tiên cần trỏ tới thư mục làm việc $GOPATH/....
        - Sau đó mới tạo 1 thư mục chứa code (hoặc clone về).

- Bước 5: Push code lên gitlab và kiểm tra