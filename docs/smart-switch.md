# 智能开关代码片段

## 开关检测逻辑
```lua
openFlag = false
switchTimmer = tmr.create()
switchTimmer:register(50, tmr.ALARM_AUTO, function()
    local value = gpio.read(1)
    if (value == 0) then
        if (not openFlag) then
            openFlag = true
            print("switch open")
            http.post('http://274n993w87.zicp.vip/switch/updateState',
                'Content-Type: application/json\r\n',
                '{"state": true}',
                function(code, data)
                    if (code < 0) then
                        print("HTTP request failed")
                    else
                        print(code, data)
                    end
                end)
        end
    else
        if (openFlag) then
            openFlag = false
            print("switch close")
            http.post('http://274n993w87.zicp.vip/switch/updateState',
                'Content-Type: application/json\r\n',
                '{"state": false}',
                function(code, data)
                    if (code < 0) then
                        print("HTTP request failed")
                    else
                        print(code, data)
                    end
                end)
        end
    end
end)
switchTimmer:start()


```