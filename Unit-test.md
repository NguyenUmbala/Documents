# UNIT TEST

- Golang có package "testing" để phục vụ cho việc test.

## Các bước thực hiện unit test:

- Bước 1: Tạo testcase.
    - Example:
        ```
        var tests = []struct {
            input int
            expected int
        }{
            {
                input -3,
                expected 3,
            },
            {
                input 5,
                expected 5,
            },
        }
        ```

- Bước 2: Kiểm tra lần lượt các testcase. Nếu sai thì xuất thông báo lỗi.
    - Example:
        ```
        for _, test := range tests {
            output := Abs(test.input)
            if output != test.expected {
                t.Error("Test failed:","\n Input:", test.input, "\n Output:", test.output, "\n Expected:", test.expected)
            }
        }
        ```

## Examples:

- Example 1: Test simple function
    ```
    func Add(a, b int) int {
        return a + b   
    }

    func TestAdd(t *testing.T) {
        var test = []struct {
            input1 int
            input2 int
            expected int 
        }{
            {
                input1 3,
                input2 4,
                expected 7,
            },
            {
                input1 2,
                input2 4,
                expected 6,
            },
        }

        for _, test := range tests {
            output := Add(test.input1, test.input2)
            
            if output != test.expected {
                t.Error("Test failed:\n Input1:", test.input1, "\n Input2:", test.input2, "\n Output:", output, "\n Expected:", test.expected)
            }
        }
    }
    ```

- Example 2: Test router
    ```
    func SetupRouters() *gin.Engine {
        var Routers *gin.Engine
        Routers = gin.Default()

        Routers.GET("/api/hello", controllers.SayHello)
        Routers.GET("/api/goodbye", controllers.SayGoodbye)

        return Routers
    }

    func TestSetupRouters(t *testing.T) {
        var tests = []struct {
            input string,
            expected gin.H,
        }{
            {
                input: "/api/hello",
                expected: gin.H {
                    "Data": "Hello",
                },
            },
            {
                input: "/api/goodbye",
                expected: gin.H {
                    "Data": "Goodbye",
                },
            },
        }
        
        // assert tương tự như testing, chỉ thay đổi cách hiển thị (link: github.com/stretchr/testify/assert)
        for _, test := range tests {
            w := performRequest(router, "GET", test.input)
            assert.Equal(t, http.StatusOK, w.Code)

            var response map[string][]string
            err := json.Unmarshal([]byte(w.Body.String()), &response)
            assert.Nil(t, err)

            data, exists := response["Data"]
            assert.True(t, exists)
            assert.Equal(t, test.expected, data)
        }
    }

    func performRequest(r http.Handler, method, path string) *httptest.ResponseRecorder {
        req, _ := http.NewRequest(method, path, nil)
        w := httptest.NewRecorder()
        r.ServeHTTP(w, req)
        return w
    }
    ```