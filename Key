getgenv().WebSocket = nil
getgenv().Websocket = nil

local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()
local api = loadstring(game:HttpGet("https://sdkapi-public.luarmor.net/library.lua"))()

api.script_id = "acdced2352def79bef163e2db39d2ec5"

local function checkKey(inputKey)
  inputKey = inputKey:gsub("%s+", "")
  if #inputKey ~= 32 then return false end

  local status = api.check_key(inputKey)

  if status.code == "KEY_VALID" then
    writefile("FireScripts/Key.txt", inputKey)

    Fluent:Notify({
      Title = "✅ Key Valid!",
      Content = "Seconds left: " .. math.abs(os.time() - status.data.auth_expire),
      Duration = 8
    })

    getgenv().script_key = inputKey

    -- Esta línea funciona porque ahora `load_script` EXISTE:
    task.spawn(function() api.load_script() end)

    return true
  else
    Fluent:Notify({ Title = "❌ Invalid Key", Content = status.code, Duration = 8 })
  end
end

-- Prueba key guardada
if isfolder("FireScripts") and isfile("FireScripts/Key.txt") then
  local key = readfile("FireScripts/Key.txt")
  if #key == 32 and checkKey(key) then return end
end

-- Si no hay key guardada → muestra la UI para pegarla
local Window = Fluent:CreateWindow({
  Title = "Fire Hub Key",
  SubTitle = "discord.gg/TU_DISCORD",
  Size = UDim2.fromOffset(500, 300),
  Theme = "Dark"
})

local Tab = Window:AddTab({ Title = "Key System 🔑" })

local keyHolder = { key = "" }

Tab:AddInput("KeyInput", {
  Title = "Paste Key Here",
  Placeholder = "Enter your 32-char key",
  Callback = function(Value)
    keyHolder.key = Value
  end
})

Tab:AddButton({
  Title = "✅ Check Key",
  Callback = function()
    if checkKey(keyHolder.key) then
      Window:Destroy()
    end
  end
})

Window:SelectTab(1)
