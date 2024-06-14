local dxfont0_fonte = dxCreateFont("font/fonte.ttf", 11) 
local dxfont1_fonte = dxCreateFont("font/fonte.ttf", 13) 
atmTxd = engineLoadTXD("models/atm/kmb_atmx.txd")
engineImportTXD(atmTxd, 2942)
bankTxd = engineLoadTXD("models/bank/lanblokd.txd")
engineImportTXD(bankTxd, 4005)

--[[local bankBot = createPed(150, 359.71246, 173.56975, 1008.38281, -90)
local atm1 = createObject(2942, 359.86437, 188.99635, 1008.04281)
local atm2 = createObject(2942, 360.86437, 188.99635, 1008.04281)
local atm3 = createObject(2942, 361.86437, 188.99635, 1008.04281)--]]
local atm4 = createObject(2942, 1928.58215, -1768.56689, 13.14688, 0, 0, 90)
local atm5 = createObject(2942, 1815.18152, -1557.53162, 13.08579, 0, 0, 70)
local atm6 = createObject(2942, 1682.24341, -1272.46252, 14.41477, 0, 0, 0)
local atm7 = createObject(2942, 1051.96143, -1131.20642, 23.42813, 0, 0, 0)
local atm8 = createObject(2942, 537.36407, -1740.75659, 11.87771, 0, 0, 180)

--[[setElementInterior(atm1, 3)
setElementDimension(atm1, 2)
setElementInterior(atm2, 3)
setElementDimension(atm2, 2)
setElementInterior(atm3, 3)
setElementDimension(atm3, 2)
setElementInterior(bankBot, 3)
setElementDimension(bankBot, 2)
setElementFrozen(bankBot, true)--]]

function Abrir_Banco ( )
    if not isEventHandlerAdded("onClientRender", root, caixaUI) then  
        if not isEventHandlerAdded("onClientRender", root, depositUI) then
            if not isEventHandlerAdded("onClientRender", root, transUI) then
                if not isEventHandlerAdded("onClientRender", root, acessingUI) then
                    if not isEventHandlerAdded("onClientRender", root, sacUI) then
                        addEventHandler("onClientRender", root, acessingUI)
                        setElementFrozen(localPlayer, true)
                        setTimer(function()
                            if not isEventHandlerAdded("onClientRender", root, caixaUI) then
                                addEventHandler("onClientRender", root, caixaUI)
                            end
                            removeEventHandler("onClientRender", root, acessingUI)
                            showCursor(true)
                            setElementFrozen(localPlayer, false)
                            setElementData(localPlayer, "Notification", false)
                            setElementData(localPlayer, "Notification:S", false)
                            showChat(false)
                        end, 2500, 1)
                    end
                end
            end
        end
    end
end
addEvent ( "AirNewSCR_AbrirBanco", true)
addEventHandler ( "AirNewSCR_AbrirBanco", root, Abrir_Banco )

function Fechar_Banco ( )
    if isEventHandlerAdded("onClientRender", root, caixaUI) then  
		showCursor(false)
        showChat(true)
        setElementData(localPlayer, "Notification", false)
        setElementData(localPlayer, "Notification:S", false)
        playSound("sfx/hit.mp3", false)
        removeEventHandler("onClientRender", root, caixaUI)
    end
    if isEventHandlerAdded("onClientRender", root, transUI) then  
        showCursor(false)
        setElementData(localPlayer, "Notification", false)
        setElementData(localPlayer, "Notification:S", false)
        showChat(true)
        playSound("sfx/hit.mp3", false)
        grid:SetVisible(false)
        removeEventHandler("onClientRender", root, transUI)
        changeVisibility("1", false)
    end
    if isEventHandlerAdded("onClientRender", root, depositUI) then  
        showCursor(false)
        showChat(true)
        playSound("sfx/hit.mp3", false)
        setElementData(localPlayer, "Notification", false)
        setElementData(localPlayer, "Notification:S", false)
        changeVisibility("2", false)
        removeEventHandler("onClientRender", root, depositUI)
    end
    if isEventHandlerAdded("onClientRender", root, sacUI) then  
        showCursor(false)
        setElementData(localPlayer, "Notification", false)
        setElementData(localPlayer, "Notification:S", false)
        showChat(true)
        playSound("sfx/hit.mp3", false)
        removeEventHandler("onClientRender", root, sacUI)
        changeVisibility("3", false)
    end
    if isEventHandlerAdded("onClientRender", root, sacUI) then  
        playSound("sfx/hit.mp3", false)
        removeEventHandler("onClientRender", root, sacUI)
        addEventHandler("onClientRender", root, caixaUI)
        changeVisibility("3", false)
    end
    if isEventHandlerAdded("onClientRender", root, depositUI) then  
        playSound("sfx/hit.mp3", false)
        removeEventHandler("onClientRender", root, depositUI)
        addEventHandler("onClientRender", root, caixaUI)
        changeVisibility("2", false)
    end
end
addEvent ( "AirNewSCR_FecharBanco", true)
addEventHandler ( "AirNewSCR_FecharBanco", root, Fechar_Banco )

--addEventHandler ( "onClientRender", getRootElement ( ), caixaUI )

function convertNumber ( number )   
    local formatted = number   
    while true do       
        formatted, k = string.gsub(formatted, "^(-?%d+)(%d%d%d)", '%1,%2')     
        if ( k==0 ) then       
            break   
        end   
    end   
    return formatted 
end

function isEventHandlerAdded( sEventName, pElementAttachedTo, func )
    if 
        type( sEventName ) == 'string' and 
        isElement( pElementAttachedTo ) and 
        type( func ) == 'function' 
    then
        local aAttachedFunctions = getEventHandlers( sEventName, pElementAttachedTo )
        if type( aAttachedFunctions ) == 'table' and #aAttachedFunctions > 0 then
            for i, v in ipairs( aAttachedFunctions ) do
                if v == func then
                    return true
                end
            end
        end
    end

    return false
end

function dxDrawLinedRectangle( x, y, width, height, color, _width, postGUI )
    _width = _width or 1
    dxDrawLine ( x, y, x+width, y, color, _width, postGUI ) -- Top
    dxDrawLine ( x, y, x, y+height, color, _width, postGUI ) -- Left
    dxDrawLine ( x, y+height, x+width, y+height, color, _width, postGUI ) -- Bottom
    return dxDrawLine ( x+width, y, x+width, y+height, color, _width, postGUI ) -- Right
end

local szx,szy = guiGetScreenSize()
local tx, ty, tz = 1411.80339, -1699.87390, 13.53949
local sz = tz-2.5
local marker = createMarker (tx, ty, sz, "cylinder", 1, 241, 155, 0, 0)
local screenW, screenH = guiGetScreenSize()
local resW, resH = 1360,768
local x, y = (screenW/resW), (screenH/resH)
local l_0_1 = false
local l_0_2, l_0_3 = guiGetScreenSize()
local l_0_4 = dxCreateScreenSource(l_0_2, l_0_3)

grid = dxGrid:Create(x*497, y*406, x*325, y*139)
colum = grid:AddColumn("Jogadores", x*200)
grid:SetVisible(false)

function playersGrid()
    grid:Clear()
    for i, player in pairs(getElementsByType("player")) do
        if (player ~= localPlayer) then
            grid:AddItem(colum, getPlayerName(player):gsub("#%x%x%x%x%x%x", ""))
        end
    end
end
addEventHandler("onClientPlayerQuit", root, playersGrid)
addEventHandler("onClientPlayerJoin", root, playersGrid)
addEventHandler("onClientResourceStart", resourceRoot, playersGrid)
addEventHandler("onClientPlayerChangeNick", root, playersGrid)

addEventHandler("onClientRender", root, function()
  if l_0_1 then
    dxUpdateScreenSource(l_0_4)
    dxDrawImage(0, 0, l_0_2, l_0_3, l_0_4)
  end
end
)


addEventHandler ( "onClientRender", root,
function ( )
         vx, vy, vz = getElementPosition(marker)
         scX, scY = getScreenFromWorldPosition(vx, vy, vz+3.5)
         cx,cy,cz,clx,cly,clz,crz,cfov = getCameraMatrix()
         dist = getDistanceBetweenPoints3D(cx, cy, cz, vx, vy, vz+3.5)
        if scX then
                 largura, altura = 626, 350
                 Tx = scX-(5000/dist)*szx/626/largura*cfov
                 Ty = scY-(100/dist)*szy/350/altura*cfov
                 Tw = (5000/dist)*szx/400/largura*cfov
                 Th = (5000/dist)*szy/400/altura*cfov 
                 if dist < 80.0 then
                        if (isLineOfSightClear(cx, cy, cz, vx, vy, vz+1.5, true, false, false)) then
                                if not isElementWithinMarker(localPlayer, marker) then
                                    --dxDrawText("$", Tx, Ty, Tw, Th + 40, tocolor(255, 255, 255, math.abs(math.sin(getTickCount()/700))*200), 1.00, dxfont1_fonte, "left", "top", false, false, true, true, false) 
                                    --dxDrawText("Loja de #f19b00vida #ffffffe #f19b00colete", Tx, Ty + 120, Tw, Th + 40, tocolor(255, 255, 255, math.abs(math.sin(getTickCount()/700))*200), 1.00, dxfont2_fonte, "left", "top", false, false, true, true, false) 
                                     --dxDrawImage ( Tx, Ty, Tw, Th, "gfx/joinIcon.png",0,0,0,tocolor(255,255,255,255))
                                    
                                end
                                
                        end
                end
        end
end)


--function cancelPedDamage(attacker)
--  cancelEvent() 
--end
--addEventHandler("onClientPedDamage", bankBot, cancelPedDamage)


function caixaUI()
    local money = convertNumber(getPlayerMoney(localPlayer)) 
    local bankMoney = convertNumber(getElementData(localPlayer, "Bank:Caixa")) 
    --exports["[VZR]Blur2"]:dxDrawBluredRectangle(0, 0, screenW, screenH, tocolor(255, 255, 255, 255))
    dxDrawImage(x*0, y*0, x*1360, y*768,"gfx/ui/bg.png", 0, 0, 0, tocolor(255, 255, 255, 255), false)
    dxDrawImage(x*0, y*0, x*1360, y*768,"gfx/ui/bg2.png", 0, 0, 0, tocolor(255, 255, 255, 255), false)
    dxDrawImage(x*0, y*0, x*1360, y*768, isCursorOnElement(x*464, y*366, x*93, y*95) and "gfx/ui/t_button2.png" or "gfx/ui/t_button.png", 0, 0, 0, tocolor(255, 255, 255, 255), false)
    dxDrawImage(x*0, y*0, x*1360, y*768, isCursorOnElement(x*625, y*366, x*93, y*95) and "gfx/ui/d_button2.png" or "gfx/ui/d_button.png", 0, 0, 0, tocolor(255, 255, 255, 255), false)
    dxDrawImage(x*0, y*0, x*1360, y*768, isCursorOnElement(x*778, y*366, x*93, y*95) and "gfx/ui/r_button2.png" or "gfx/ui/r_button.png", 0, 0, 0, tocolor(255, 255, 255, 255), false)
    dxDrawText(getPlayerName(localPlayer):gsub("#%x%x%x%x%x%x", ""), x*310, y*158, x*406, y*182, tocolor(0, 0, 0, 127), 1.00, dxfont0_fonte, "left", "top", false, false, false, true, false)
    dxDrawText("R$ "..money, x*339, y*189, x*426, y*213, tocolor(0, 0, 0, 127), 1.00, dxfont0_fonte, "left", "top", false, false, false, false, false)
    dxDrawText("R$ "..bankMoney, x*340, y*216, x*427, y*240, tocolor(0, 0, 0, 127), 1.00, dxfont0_fonte, "left", "top", false, false, false, false, false)
    if getElementData(localPlayer, "Notification") then
        dxDrawText("[Erro] "..getElementData(localPlayer, "Notification"), x*240, y*680, x*706, y*441, tocolor(255, 0, 0, 255), 1.00, dxfont0_fonte, "left", "top", false, false, false, false, false)
    end
    if getElementData(localPlayer, "Notification:S") then
        dxDrawText("[Sucesso] "..getElementData(localPlayer, "Notification:S"), x*240, y*680, x*706, y*441, tocolor(0, 255, 0, 255), 1.00, dxfont0_fonte, "left", "top", false, false, false, false, false)
    end
end

function acessingUI()
   --exports["[VZR]Blur2"]:dxDrawBluredRectangle(0, 0, screenW, screenH, tocolor(255, 255, 255, 255))
    dxDrawImage(x*0, y*0, x*1360, y*768,"gfx/ui/bg1.png", 0, 0, 0, tocolor(255, 255, 255, 255), false)
end

function depositUI()
    local money = convertNumber(getPlayerMoney(localPlayer)) 
    local bankMoney = convertNumber(getElementData(localPlayer, "Bank:Caixa"))
    --exports["[VZR]Blur2"]:dxDrawBluredRectangle(0, 0, screenW, screenH, tocolor(255, 255, 255, 255))
    dxDrawImage(x*0, y*0, x*1360, y*768,"gfx/ui/bg.png", 0, 0, 0, tocolor(255, 255, 255, 255), false)
    dxDrawImage(x*0, y*0, x*1360, y*768,"gfx/ui/bgd.png", 0, 0, 0, tocolor(255, 255, 255, 255), false)
    dxDrawText(getPlayerName(localPlayer):gsub("#%x%x%x%x%x%x", ""), x*310, y*158, x*406, y*182, tocolor(0, 0, 0, 127), 1.00, dxfont0_fonte, "left", "top", false, false, false, true, false)
    dxDrawText("R$ "..money, x*339, y*189, x*426, y*213, tocolor(0, 0, 0, 127), 1.00, dxfont0_fonte, "left", "top", false, false, false, false, false)
    dxDrawText("R$ "..bankMoney, x*340, y*216, x*427, y*240, tocolor(0, 0, 0, 127), 1.00, dxfont0_fonte, "left", "top", false, false, false, false, false)
    dxDrawRectangle(x*402, y*564, x*540, y*56, isCursorOnElement(x*502, y*564, x*320, y*56) and tocolor(0, 84, 131, 255) or tocolor(0, 112, 175, 255), false)
    createEditBox("2", 0.379, 0.443, 0.22, 0.06, true, "", false, 7, "arial", false, 1, {0, 0, 0, 127 }, true, { 0, 0, 0, 55 }, 1, true, 60, true, "Digite o valor", { 0, 0, 0, 127 }, true, 1, "arial", true, true, {0, 0, 0}, false) 
     dxDrawText("   Cancelar", x*619, y*580, x*706, y*441, --[[isCursorOnElement(x*502, y*401, x*320, y*56) and]] tocolor(255, 255, 255, 255)--[[ or tocolor(0, 0, 0, 127)]], 1.00, dxfont1_fonte, "left", "top", false, false, false, false, false)
    dxDrawRectangle(x*402, y*480, x*540, y*56, isCursorOnElement(x*502, y*480, x*320, y*56) and tocolor(0, 84, 131, 255) or tocolor(0, 112, 175, 255), false)
    dxDrawText("   Depositar", x*619, y*497, x*706, y*441, --[[isCursorOnElement(x*502, y*401, x*320, y*56) and]] tocolor(255, 255, 255, 255)--[[ or tocolor(0, 0, 0, 127)]], 1.00, dxfont1_fonte, "left", "top", false, false, false, false, false)
    if getElementData(localPlayer, "Notification") then
        dxDrawText("[Erro] "..getElementData(localPlayer, "Notification"), x*240, y*680, x*706, y*441, tocolor(255, 0, 0, 255), 1.00, dxfont0_fonte, "left", "top", false, false, false, false, false)
    end
    if getElementData(localPlayer, "Notification:S") then
        dxDrawText("[Sucesso] "..getElementData(localPlayer, "Notification:S"), x*240, y*680, x*706, y*441, tocolor(0, 255, 0, 255), 1.00, dxfont0_fonte, "left", "top", false, false, false, false, false)
    end
end

function sacUI()
    local money = convertNumber(getPlayerMoney(localPlayer)) 
    local bankMoney = convertNumber(getElementData(localPlayer, "Bank:Caixa"))
    --exports["[VZR]Blur2"]:dxDrawBluredRectangle(0, 0, screenW, screenH, tocolor(255, 255, 255, 255))
    dxDrawImage(x*0, y*0, x*1360, y*768,"gfx/ui/bg.png", 0, 0, 0, tocolor(255, 255, 255, 255), false)
    dxDrawImage(x*0, y*0, x*1360, y*768,"gfx/ui/bgr.png", 0, 0, 0, tocolor(255, 255, 255, 255), false)
    dxDrawText(getPlayerName(localPlayer):gsub("#%x%x%x%x%x%x", ""), x*310, y*158, x*406, y*182, tocolor(0, 0, 0, 127), 1.00, dxfont0_fonte, "left", "top", false, false, false, true, false)
    dxDrawText("R$ "..money, x*339, y*189, x*426, y*213, tocolor(0, 0, 0, 127), 1.00, dxfont0_fonte, "left", "top", false, false, false, false, false)
    dxDrawText("R$ "..bankMoney, x*340, y*216, x*427, y*240, tocolor(0, 0, 0, 127), 1.00, dxfont0_fonte, "left", "top", false, false, false, false, false)
    dxDrawRectangle(x*402, y*564, x*540, y*56, isCursorOnElement(x*502, y*564, x*320, y*56) and tocolor(0, 84, 131, 255) or tocolor(0, 112, 175, 255), false)
    createEditBox("3", 0.379, 0.443, 0.22, 0.06, true, "", false, 7, "arial", false, 1, {0, 0, 0, 127 }, true, { 0, 0, 0, 55 }, 1, true, 60, true, "Digite o valor", { 0, 0, 0, 127 }, true, 1, "arial", true, true, {0, 0, 0}, false) 
    dxDrawText("   Cancelar", x*619, y*580, x*706, y*441, --[[isCursorOnElement(x*502, y*401, x*320, y*56) and]] tocolor(255, 255, 255, 255)--[[ or tocolor(0, 0, 0, 127)]], 1.00, dxfont1_fonte, "left", "top", false, false, false, false, false)
    dxDrawRectangle(x*402, y*480, x*540, y*56, isCursorOnElement(x*502, y*480, x*320, y*56) and tocolor(0, 84, 131, 255) or tocolor(0, 112, 175, 255), false)
    dxDrawText("   Retirar", x*619, y*497, x*706, y*441, --[[isCursorOnElement(x*502, y*401, x*320, y*56) and]] tocolor(255, 255, 255, 255)--[[ or tocolor(0, 0, 0, 127)]], 1.00, dxfont1_fonte, "left", "top", false, false, false, false, false)
    if getElementData(localPlayer, "Notification") then
        dxDrawText("[Erro] "..getElementData(localPlayer, "Notification"), x*240, y*680, x*706, y*441, tocolor(255, 0, 0, 255), 1.00, dxfont0_fonte, "left", "top", false, false, false, false, false)
    end
    if getElementData(localPlayer, "Notification:S") then
        dxDrawText("[Sucesso] "..getElementData(localPlayer, "Notification:S"), x*240, y*680, x*706, y*441, tocolor(0, 255, 0, 255), 1.00, dxfont0_fonte, "left", "top", false, false, false, false, false)
    end
end

function transUI()
    local money = convertNumber(getPlayerMoney(localPlayer)) 
    local bankMoney = convertNumber(getElementData(localPlayer, "Bank:Caixa"))
    --exports["[VZR]Blur2"]:dxDrawBluredRectangle(0, 0, screenW, screenH, tocolor(255, 255, 255, 255))
    dxDrawImage(x*0, y*0, x*1360, y*768,"gfx/ui/bg.png", 0, 0, 0, tocolor(255, 255, 255, 255), false)
    dxDrawImage(x*0, y*0, x*1360, y*768,"gfx/ui/bgt.png", 0, 0, 0, tocolor(255, 255, 255, 255), false)
    dxDrawText(getPlayerName(localPlayer):gsub("#%x%x%x%x%x%x", ""), x*310, y*158, x*406, y*182, tocolor(0, 0, 0, 127), 1.00, dxfont0_fonte, "left", "top", false, false, false, true, false)
    dxDrawText("R$ "..money, x*339, y*189, x*426, y*213, tocolor(0, 0, 0, 127), 1.00, dxfont0_fonte, "left", "top", false, false, false, false, false)
    dxDrawText("R$ "..bankMoney, x*340, y*216, x*427, y*240, tocolor(0, 0, 0, 127), 1.00, dxfont0_fonte, "left", "top", false, false, false, false, false)
    dxDrawRectangle(x*402, y*564, x*540, y*56, isCursorOnElement(x*502, y*564, x*320, y*56) and tocolor(0, 84, 131, 255) or tocolor(0, 112, 175, 255), false)
    createEditBox("1", 0.379, 0.443, 0.22, 0.06, true, "", false, 7, "arial", false, 1, {0, 0, 0, 127 }, true, { 0, 0, 0, 55 }, 1, true, 60, true, "Digite o valor", { 0, 0, 0, 127 }, true, 1, "arial", true, true, {0, 0, 0}, false) 
    dxDrawText("Transferir", x*619, y*580, x*706, y*441, --[[isCursorOnElement(x*502, y*401, x*320, y*56) and]] tocolor(255, 255, 255, 255)--[[ or tocolor(0, 0, 0, 127)]], 1.00, dxfont1_fonte, "left", "top", false, false, false, false, false)
    if getElementData(localPlayer, "Notification") then
        dxDrawText("[Erro] "..getElementData(localPlayer, "Notification"), x*240, y*680, x*706, y*441, tocolor(255, 0, 0, 255), 1.00, dxfont0_fonte, "left", "top", false, false, false, false, false)
    end
    if getElementData(localPlayer, "Notification:S") then
        dxDrawText("[Sucesso] "..getElementData(localPlayer, "Notification:S"), x*240, y*680, x*706, y*441, tocolor(0, 255, 0, 255), 1.00, dxfont0_fonte, "left", "top", false, false, false, false, false)
    end
end

--[[function render()
local screenx, screeny, worldx, worldy, worldz = getCursorPosition()
local px, py, pz = getCameraMatrix()
local hit, x, y, z, elementHit = processLineOfSight ( px, py, pz, worldx, worldy, worldz )
local tx, ty, tz = getElementPosition(localPlayer) 
--local rx, ry, rz = getElementPosition(atm1) 
--local rx2, ry2, rz2 = getElementPosition(atm2) 
--local rx3, ry3, rz3 = getElementPosition(atm3) 
local rx4, ry4, rz4 = getElementPosition(atm4) 
local rx5, ry5, rz5 = getElementPosition(atm5) 
local rx6, ry6, rz6 = getElementPosition(atm6) 
local rx7, ry7, rz7 = getElementPosition(atm7) 
local rx8, ry8, rz8 = getElementPosition(atm8) 
--local distancia = getDistanceBetweenPoints3D(tx, ty, tz, rx, ry, rz) 
--local distancia2 = getDistanceBetweenPoints3D(tx, ty, tz, rx2, ry2, rz2) 
--local distancia3 = getDistanceBetweenPoints3D(tx, ty, tz, rx3, ry3, rz3) 
local distancia4 = getDistanceBetweenPoints3D(tx, ty, tz, rx4, ry4, rz4)
local distancia5 = getDistanceBetweenPoints3D(tx, ty, tz, rx5, ry5, rz5) 
local distancia6 = getDistanceBetweenPoints3D(tx, ty, tz, rx6, ry6, rz6) 
local distancia7 = getDistanceBetweenPoints3D(tx, ty, tz, rx7, ry7, rz7) 
local distancia8 = getDistanceBetweenPoints3D(tx, ty, tz, rx8, ry8, rz8) 

    if not isEventHandlerAdded("onClientRender", root, caixaUI) then
        if (distancia <= 1.5)  then 
            if not isEventHandlerAdded("onClientRender", root, caixaUI) then
                if not isEventHandlerAdded("onClientRender", root, depositUI) then
                    if not isEventHandlerAdded("onClientRender", root, sacUI) then
                        if not isEventHandlerAdded("onClientRender", root, transUI) then
                        if not isEventHandlerAdded("onClientRender", root, acessingUI) then
                            if hit then
                                if elementHit == atm1 then
                                    addEventHandler("onClientRender", root, acessingUI)
                                    setElementFrozen(localPlayer, true)
                                    setTimer(function()
                                        if not isEventHandlerAdded("onClientRender", root, caixaUI) then
                                            addEventHandler("onClientRender", root, caixaUI)
                                        end
                                        removeEventHandler("onClientRender", root, acessingUI)
                                        showCursor(true)
                                        setElementFrozen(localPlayer, false)
                                        setElementData(localPlayer, "Notification", false)
                                        setElementData(localPlayer, "Notification:S", false)
                                        showChat(false)
                                    end, 2500, 1)
                                end 
                            end
                        end  
                    end
                    end
                end
            end
        end
        if (distancia2 <= 1.5)  then 
            if not isEventHandlerAdded("onClientRender", root, caixaUI) then
                if hit then
                    if not isEventHandlerAdded("onClientRender", root, depositUI) then
                        if not isEventHandlerAdded("onClientRender", root, transUI) then
                        if not isEventHandlerAdded("onClientRender", root, acessingUI) then
                            if not isEventHandlerAdded("onClientRender", root, sacUI) then
                                if elementHit == atm2 then
                                    addEventHandler("onClientRender", root, acessingUI)
                                    setElementFrozen(localPlayer, true)
                                    setTimer(function()
                                        if not isEventHandlerAdded("onClientRender", root, caixaUI) then
                                            addEventHandler("onClientRender", root, caixaUI)
                                        end
                                        removeEventHandler("onClientRender", root, acessingUI)
                                        showCursor(true)
                                        setElementFrozen(localPlayer, false)
                                        setElementData(localPlayer, "Notification", false)
                                        setElementData(localPlayer, "Notification:S", false)
                                        showChat(false)
                                    end, 2500, 1)
                                end 
                            end
                        end
                        end
                    end
                end  
            end
        end
        if (distancia3 <= 1.5)  then 
            if not isEventHandlerAdded("onClientRender", root, caixaUI) then  
                if not isEventHandlerAdded("onClientRender", root, depositUI) then
                    if not isEventHandlerAdded("onClientRender", root, transUI) then
                        if not isEventHandlerAdded("onClientRender", root, sacUI) then
                        if not isEventHandlerAdded("onClientRender", root, acessingUI) then
                            if hit then
                                if elementHit == atm3 then
                                    addEventHandler("onClientRender", root, acessingUI)
                                    setElementFrozen(localPlayer, true)
                                    setTimer(function()
                                        if not isEventHandlerAdded("onClientRender", root, caixaUI) then
                                            addEventHandler("onClientRender", root, caixaUI)
                                        end
                                        removeEventHandler("onClientRender", root, acessingUI)
                                        showCursor(true)
                                        setElementFrozen(localPlayer, false)
                                        setElementData(localPlayer, "Notification", false)
                                        setElementData(localPlayer, "Notification:S", false)
                                        showChat(false)
                                    end, 2500, 1)
                                end 
                            end 
                        end
                        end
                    end
                end
            end 
        end
        if (distancia4 <= 1.5)  then 
            if not isEventHandlerAdded("onClientRender", root, caixaUI) then  
                if not isEventHandlerAdded("onClientRender", root, depositUI) then
                    if not isEventHandlerAdded("onClientRender", root, transUI) then
                    if not isEventHandlerAdded("onClientRender", root, acessingUI) then
                        if not isEventHandlerAdded("onClientRender", root, sacUI) then
                            if hit then
                                if elementHit == atm4 then
                                    addEventHandler("onClientRender", root, acessingUI)
                                    setElementFrozen(localPlayer, true)
                                    setTimer(function()
                                        if not isEventHandlerAdded("onClientRender", root, caixaUI) then
                                            addEventHandler("onClientRender", root, caixaUI)
                                        end
                                        removeEventHandler("onClientRender", root, acessingUI)
                                        showCursor(true)
                                        setElementFrozen(localPlayer, false)
                                        setElementData(localPlayer, "Notification", false)
                                        setElementData(localPlayer, "Notification:S", false)
                                        showChat(false)
                                    end, 2500, 1)
                                end 
                            end 
                        end
                        end
                    end
                end
            end
        end
        if (distancia5 <= 1.5)  then 
            if not isEventHandlerAdded("onClientRender", root, caixaUI) then  
                if not isEventHandlerAdded("onClientRender", root, depositUI) then
                    if not isEventHandlerAdded("onClientRender", root, transUI) then
                    if not isEventHandlerAdded("onClientRender", root, acessingUI) then
                        if not isEventHandlerAdded("onClientRender", root, sacUI) then
                            if hit then
                                if elementHit == atm5 then
                                    addEventHandler("onClientRender", root, acessingUI)
                                    setElementFrozen(localPlayer, true)
                                    setTimer(function()
                                        if not isEventHandlerAdded("onClientRender", root, caixaUI) then
                                            addEventHandler("onClientRender", root, caixaUI)
                                        end
                                        removeEventHandler("onClientRender", root, acessingUI)
                                        showCursor(true)
                                        setElementFrozen(localPlayer, false)
                                        setElementData(localPlayer, "Notification", false)
                                        setElementData(localPlayer, "Notification:S", false)
                                        showChat(false)
                                    end, 2500, 1)
                                end 
                            end 
                        end
                        end
                    end
                end
            end 
        end
        if (distancia6 <= 1.5)  then 
            if not isEventHandlerAdded("onClientRender", root, caixaUI) then  
                if not isEventHandlerAdded("onClientRender", root, depositUI) then
                    if not isEventHandlerAdded("onClientRender", root, transUI) then
                    if not isEventHandlerAdded("onClientRender", root, acessingUI) then
                        if not isEventHandlerAdded("onClientRender", root, sacUI) then
                            if hit then
                                if elementHit == atm6 then
                                    addEventHandler("onClientRender", root, acessingUI)
                                    setElementFrozen(localPlayer, true)
                                    setTimer(function()
                                        if not isEventHandlerAdded("onClientRender", root, caixaUI) then
                                            addEventHandler("onClientRender", root, caixaUI)
                                        end
                                        removeEventHandler("onClientRender", root, acessingUI)
                                        showCursor(true)
                                        setElementFrozen(localPlayer, false)
                                        setElementData(localPlayer, "Notification", false)
                                        setElementData(localPlayer, "Notification:S", false)
                                        showChat(false)
                                    end, 2500, 1)
                                end 
                            end 
                        end
                        end
                    end
                end
            end 
        end
        if (distancia7 <= 1.5)  then 
            if not isEventHandlerAdded("onClientRender", root, caixaUI) then  
                if not isEventHandlerAdded("onClientRender", root, depositUI) then
                    if not isEventHandlerAdded("onClientRender", root, transUI) then
                        if not isEventHandlerAdded("onClientRender", root, sacUI) then
                        if not isEventHandlerAdded("onClientRender", root, acessingUI) then
                            if hit then
                                if elementHit == atm7 then
                                    addEventHandler("onClientRender", root, acessingUI)
                                    setElementFrozen(localPlayer, true)
                                    setTimer(function()
                                        if not isEventHandlerAdded("onClientRender", root, caixaUI) then
                                            addEventHandler("onClientRender", root, caixaUI)
                                        end
                                        removeEventHandler("onClientRender", root, acessingUI)
                                        showCursor(true)
                                        setElementFrozen(localPlayer, false)
                                        setElementData(localPlayer, "Notification", false)
                                        setElementData(localPlayer, "Notification:S", false)
                                        showChat(false)
                                    end, 2500, 1)
                                end 
                            end
                            end 
                        end
                    end
                end
            end 
        end
        if (distancia8 <= 1.5)  then 
            if not isEventHandlerAdded("onClientRender", root, caixaUI) then  
                if not isEventHandlerAdded("onClientRender", root, depositUI) then
                    if not isEventHandlerAdded("onClientRender", root, transUI) then
                        if not isEventHandlerAdded("onClientRender", root, sacUI) then
                        if not isEventHandlerAdded("onClientRender", root, acessingUI) then
                            if hit then
                                if elementHit == atm8 then
                                    addEventHandler("onClientRender", root, acessingUI)
                                    setElementFrozen(localPlayer, true)
                                    setTimer(function()
                                        if not isEventHandlerAdded("onClientRender", root, caixaUI) then
                                            addEventHandler("onClientRender", root, caixaUI)
                                        end
                                        removeEventHandler("onClientRender", root, acessingUI)
                                        showCursor(true)
                                        setElementFrozen(localPlayer, false)
                                        setElementData(localPlayer, "Notification", false)
                                        setElementData(localPlayer, "Notification:S", false)
                                        showChat(false)
                                    end, 2500, 1)
                                end 
                            end 
                        end
                        end
                    end
                end
            end 
        end
    end
end
addEventHandler("onClientClick", root, render)--]]

local rootElement = getRootElement() 
local screenWidth, screenHeight = guiGetScreenSize() 
local maxrange = 20 

function dxDrawTextOnElement(TheElement,text,height,distance,R,G,B,alpha,size,font, ...)
    local x, y, z = getElementPosition(TheElement)
    local x2, y2, z2 = getCameraMatrix()
    local distance = distance or 20
    local height = height or 1
    local value1 = 2
    local value2 = 2

    if (isLineOfSightClear(x, y, z+2, x2, y2, z2, ...)) then
        local sx, sy = getScreenFromWorldPosition(x, y, z+height)
        if(sx) and (sy) then
            local distanceBetweenPoints = getDistanceBetweenPoints3D(x, y, z, x2, y2, z2)
            if(distanceBetweenPoints < distance) then
                dxDrawText(text, sx+value1, sy+value2, sx, sy, tocolor(R or 255, G or 255, B or 255, alpha or 255), (size or 1)-(distanceBetweenPoints / distance), font or "arial", "center", "center", false, false, false, true, false)
            end
        end
    end
end

function wolrdTexts()
    --dxDrawTextOnElement(bankBot,"Atendente #1066e7(BOT)",1,20,255,255,255,255,1.5,"default")
    --dxDrawTextOnElement(atm1,"Clique para utilizar ",1,20,255,255,255,255,1.5,"default")
    --dxDrawTextOnElement(atm2,"Clique para utilizar ",1,20,255,255,255,255,1.5,"default")
    --dxDrawTextOnElement(atm3,"Clique para utilizar ",1,20,255,255,255,255,1.5,"default")
    --dxDrawTextOnElement(atm4,"Clique para utilizar ",1,20,255,255,255,255,1.5,"default")
    --dxDrawTextOnElement(atm5,"Clique para utilizar ",1,20,255,255,255,255,1.5,"default")
    --dxDrawTextOnElement(atm6,"Clique para utilizar ",1,20,255,255,255,255,1.5,"default")
    --dxDrawTextOnElement(atm7,"Clique para utilizar ",1,20,255,255,255,255,1.5,"default")
    --dxDrawTextOnElement(atm8,"Clique para utilizar ",1,20,255,255,255,255,1.5,"default")
end
addEventHandler("onClientRender",rootElement, wolrdTexts)


function closePanel(_,state)
    if isEventHandlerAdded("onClientRender", root, caixaUI) then  
        if state == "down" then
            if isCursorOnElement(x*1068, y*94, x*36, y*38) then 
		        showCursor(false)
                showChat(true)
                setElementData(localPlayer, "Notification", false)
                setElementData(localPlayer, "Notification:S", false)
                playSound("sfx/hit.mp3", false)
                removeEventHandler("onClientRender", root, caixaUI)
            end
        end
    end
    if isEventHandlerAdded("onClientRender", root, transUI) then  
        if state == "down" then
            if isCursorOnElement(x*1068, y*94, x*36, y*38) then 
                showCursor(false)
                setElementData(localPlayer, "Notification", false)
                setElementData(localPlayer, "Notification:S", false)
                showChat(true)
                playSound("sfx/hit.mp3", false)
                grid:SetVisible(false)
                removeEventHandler("onClientRender", root, transUI)
                changeVisibility("1", false)
            end
        end
    end
    if isEventHandlerAdded("onClientRender", root, depositUI) then  
        if state == "down" then
            if isCursorOnElement(x*1068, y*94, x*36, y*38) then 
                showCursor(false)
                showChat(true)
                playSound("sfx/hit.mp3", false)
                setElementData(localPlayer, "Notification", false)
                setElementData(localPlayer, "Notification:S", false)
                changeVisibility("2", false)
                removeEventHandler("onClientRender", root, depositUI)
            end
        end
    end
    if isEventHandlerAdded("onClientRender", root, sacUI) then  
        if state == "down" then
            if isCursorOnElement(x*1068, y*94, x*36, y*38) then 
                showCursor(false)
                setElementData(localPlayer, "Notification", false)
                setElementData(localPlayer, "Notification:S", false)
                showChat(true)
                playSound("sfx/hit.mp3", false)
                removeEventHandler("onClientRender", root, sacUI)
                changeVisibility("3", false)
            end
        end
    end


    if isEventHandlerAdded("onClientRender", root, sacUI) then  
        if state == "down" then
            if isCursorOnElement(x*502, y*564, x*320, y*56) then 
                playSound("sfx/hit.mp3", false)
                removeEventHandler("onClientRender", root, sacUI)
                addEventHandler("onClientRender", root, caixaUI)
                changeVisibility("3", false)
            end
        end
    end
    if isEventHandlerAdded("onClientRender", root, depositUI) then  
        if state == "down" then
            if isCursorOnElement(x*502, y*564, x*320, y*56) then 
                playSound("sfx/hit.mp3", false)
                removeEventHandler("onClientRender", root, depositUI)
                addEventHandler("onClientRender", root, caixaUI)
                changeVisibility("2", false)
            end
        end
    end
end
addEventHandler("onClientClick", root, closePanel)


function uiButtons(_,state)
    if isEventHandlerAdded("onClientRender", root, caixaUI) then   
        if state == "down" then
            if not isEventHandlerAdded("onClientRender", root, transUI) or isEventHandlerAdded("onClientRender", root, depositUI) or isEventHandlerAdded("onClientRender", root, sacUI) then  
                if isCursorOnElement(x*464, y*366, x*93, y*95) then -- trans
                    playSound("sfx/hit.mp3", false)
                    removeEventHandler("onClientRender", root, caixaUI)
                    addEventHandler("onClientRender", root, transUI)
                    grid:SetVisible(true)
                    changeVisibility("1", true)
                end
            end
            if not isEventHandlerAdded("onClientRender", root, depositUI) or isEventHandlerAdded("onClientRender", root, sacUI) or isEventHandlerAdded("onClientRender", root, transUI) then  
                if isCursorOnElement(x*625, y*366, x*93, y*95) then -- deposit
                    playSound("sfx/hit.mp3", false)
                    removeEventHandler("onClientRender", root, caixaUI)
                    addEventHandler("onClientRender", root, depositUI)
                    changeVisibility("2", true)
                end
            end
            if not isEventHandlerAdded("onClientRender", root, sacUI) or isEventHandlerAdded("onClientRender", root, depositUI) or isEventHandlerAdded("onClientRender", root, transUI) then  
                if isCursorOnElement(x*778, y*366, x*93, y*95) then -- sac
                    playSound("sfx/hit.mp3", false)
                    removeEventHandler("onClientRender", root, caixaUI)
                    addEventHandler("onClientRender", root, sacUI)
                    changeVisibility("3", true)
                end
            end
        end
    end
end
addEventHandler("onClientClick", root, uiButtons)

function depositButton(_,state)
    if isEventHandlerAdded("onClientRender", root, depositUI) then   
        if state == "down" then
            if isCursorOnElement(x*502, y*480, x*320, y*56) then 
                if getText("2") then
                    if tonumber(getText("2")) < getPlayerMoney(localPlayer) or tonumber(getText("2")) == getPlayerMoney(localPlayer) then
                        playSound("sfx/hit.mp3", false)
                        addEventHandler("onClientRender", root, caixaUI)
                        changeVisibility("2", false)
                        removeEventHandler("onClientRender", root, depositUI)
                        setElementData(localPlayer, "Bank:Caixa", getElementData(localPlayer, "Bank:Caixa") + tonumber(getText("2")))
                        setElementData(localPlayer, "Notification", false)
                        triggerServerEvent("onDepositMoney", localPlayer, localPlayer, tonumber(getText("2")))
                        setElementData(localPlayer, "Notification:S", "Depósito de R$ "..tonumber(getText("2")).." feito!")
                        setTimer(setElementData, 7000, 1, localPlayer, "Notification:S", false)
                    else
                        playSound("sfx/hit.mp3", false)
                        setElementData(localPlayer, "Notification:S", false)
                        setElementData(localPlayer, "Notification", "Você não possui este valor!")
                        setTimer(setElementData, 7000, 1, localPlayer, "Notification", false)
                    end
                end
            end
        end
    end
end
addEventHandler("onClientClick", root, depositButton)

function saqueButton(_,state)
    if isEventHandlerAdded("onClientRender", root, sacUI) then   
        if state == "down" then
            if isCursorOnElement(x*502, y*480, x*320, y*56) then 
                if getText("3") then
                    if tonumber(getText("3")) == getElementData(localPlayer, "Bank:Caixa") or  tonumber(getText("3")) < getElementData(localPlayer, "Bank:Caixa") then
                        playSound("sfx/hit.mp3", false)
                        addEventHandler("onClientRender", root, caixaUI)
                        removeEventHandler("onClientRender", root, sacUI)
                        changeVisibility("3", false)
                        setElementData(localPlayer, "Bank:Caixa", getElementData(localPlayer, "Bank:Caixa") - tonumber(getText("3")))
                        setElementData(localPlayer, "Notification", false)
                        triggerServerEvent("saqueBankMoney", localPlayer, localPlayer, tonumber(getText("3")))
                        setElementData(localPlayer, "Notification:S", "Retirada de R$ "..tonumber(getText("3")).." feito!")
                        setTimer(setElementData, 7000, 1, localPlayer, "Notification:S", false)
                    else
                        playSound("sfx/hit.mp3", false)
                        setElementData(localPlayer, "Notification:S", false)
                        setElementData(localPlayer, "Notification", "Você não possui este valor!")
                        setTimer(setElementData, 7000, 1, localPlayer, "Notification", false)
                    end
                end
            end
        end
    end
end
addEventHandler("onClientClick", root, saqueButton)

function transButton(_,state)
    local gridItem = grid:GetSelectedItem()
    local item = grid:GetItemDetails(colum, gridItem)
    if isEventHandlerAdded("onClientRender", root, transUI) then   
        if state == "down" then
            if isCursorOnElement(x*502, y*564, x*320, y*56) then 
               -- if getPlayerFromName(item) then
                    if getText("1") then
                        if tonumber(getText("1")) == getElementData(localPlayer, "Bank:Caixa") or  tonumber(getText("1")) < getElementData(localPlayer, "Bank:Caixa") then
                            playSound("sfx/hit.mp3", false)
                            addEventHandler("onClientRender", root, caixaUI)
                            grid:SetVisible(false)
                            changeVisibility("1", false)
                            removeEventHandler("onClientRender", root, transUI)
                            setElementData(localPlayer, "Bank:Caixa", getElementData(localPlayer, "Bank:Caixa") - tonumber(getText("1")))
                            setElementData(localPlayer, "Notification", false)
                            setElementData(localPlayer, "Notification:S", "Transferência de R$ "..tonumber(getText("1"))..", para "..item.." feito!")
                            setTimer(setElementData, 7000, 1, localPlayer, "Notification:S", false)
                            triggerServerEvent("transMoney", localPlayer, tonumber(getText("1")), item)
                        else
                            playSound("sfx/hit.mp3", false)
                            setElementData(localPlayer, "Notification:S", false)
                            setElementData(localPlayer, "Notification", "Você não possui este valor!")
                            setTimer(setElementData, 7000, 1, localPlayer, "Notification", false)
                        end
                    end
               --[[else
                    setElementData(localPlayer, "Notification:S", false)
                    setElementData(localPlayer, "Notification", "Jogador não encontrado! Verifique se ocorreu mudança no nome.")
                    setTimer(setElementData, 7000, 1, localPlayer, "Notification", false)
                end]]
            end
        end
    end
end
addEventHandler("onClientClick", root, transButton)


function isCursorOnElement( posX, posY, width, height )
  if isCursorShowing( ) then
    local mouseX, mouseY = getCursorPosition( )
    local clientW, clientH = guiGetScreenSize( )
    local mouseX, mouseY = mouseX * clientW, mouseY * clientH
    if ( mouseX > posX and mouseX < ( posX + width ) and mouseY > posY and mouseY < ( posY + height ) ) then
      return true
    end
  end
  return false
end


