cmake_minimum_required(VERSION 2.8)

include(CryptoNoteWallet.cmake)


project(${CN_PROJECT_NAME})

execute_process(COMMAND git log -1 --pretty=format:%h
WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
OUTPUT_VARIABLE GIT_REVISION)

set(CRYPTONOTE_LIB cryptonote)
set (CMAKE_PREFIX_PATH "/Users/travis/Qt5.11.1/5.11.1/")

include_directories(${CMAKE_BINARY_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}
    src
    PKGuiexternal
    PKGuiinclude
    PKGuisrc)

find_package(Qt5Gui REQUIRED)
find_package(Qt5Widgets REQUIRED)
find_package(Qt5Network REQUIRED)

if(STATIC)
  set(Boost_USE_STATIC_LIBS ON)
  set(Boost_USE_STATIC_RUNTIME ON)
endif()

#set(Boost_DEBUG on)
find_package(Boost 1.53 REQUIRED COMPONENTS system filesystem date_time chrono regex serialization program_options)
include_directories(SYSTEM ${Boost_INCLUDE_DIRS})
find_package( Threads )
if(MINGW)
  set(Boost_LIBRARIES "${Boost_LIBRARIES};ws2_32;mswsock")
elseif(APPLE)
  set(Boost_LIBRARIES "${Boost_LIBRARIES}")
elseif(NOT MSVC)
  set(Boost_LIBRARIES "${Boost_LIBRARIES};rt")
endif()

set(VERSION "")
configure_file("PKGuisrc/version.h.in" "version.h")
configure_file("src/CryptoNoteWalletConfig.h.in" "CryptoNoteWalletConfig.h")

add_definitions(-DVERSION=\"${CN_VERSION}\")
add_definitions(-DGIT_REVISION=\"${GIT_REVISION}\")

set(CMAKE_AUTOMOC ON)

set(CRYPTONOTE_SOURCES
  PKGuiexternal/miniupnpc/connecthostport.c
  PKGuiexternal/miniupnpc/igd_desc_parse.c
  PKGuiexternal/miniupnpc/minisoap.c
  PKGuiexternal/miniupnpc/miniupnpc.c
  PKGuiexternal/miniupnpc/miniwget.c
  PKGuiexternal/miniupnpc/minixml.c
  PKGuiexternal/miniupnpc/portlistingparse.c
  PKGuiexternal/miniupnpc/receivedata.c
  PKGuiexternal/miniupnpc/upnpcommands.c
  PKGuiexternal/miniupnpc/upnpreplyparse.c
  PKGuisrc/Common/Base58.cpp
  PKGuisrc/Common/CommandLine.cpp
  PKGuisrc/Common/Util.cpp
  PKGuisrc/Common/StringTools.cpp
  PKGuisrc/Common/JsonValue.cpp
  PKGuisrc/Common/ConsoleTools.cpp
  PKGuisrc/Common/MemoryInputStream.cpp
  PKGuisrc/Common/PathTools.cpp
  PKGuisrc/Common/StdInputStream.cpp
  PKGuisrc/Common/StdOutputStream.cpp
  PKGuisrc/Common/StreamTools.cpp
  PKGuisrc/Common/StringOutputStream.cpp
  PKGuisrc/Common/StringView.cpp
  PKGuisrc/Common/VectorOutputStream.cpp
  PKGuisrc/crypto/blake256.c
  PKGuisrc/crypto/chacha.c
  PKGuisrc/crypto/crypto-ops-data.c
  PKGuisrc/crypto/crypto-ops.c
  PKGuisrc/crypto/crypto.cpp
  PKGuisrc/crypto/groestl.c
  PKGuisrc/crypto/hash-extra-blake.c
  PKGuisrc/crypto/hash-extra-groestl.c
  PKGuisrc/crypto/hash-extra-jh.c
  PKGuisrc/crypto/hash-extra-skein.c
  PKGuisrc/crypto/hash.c
  PKGuisrc/crypto/jh.c
  PKGuisrc/crypto/keccak.c
  PKGuisrc/crypto/oaes_lib.c
  PKGuisrc/crypto/random.c
  PKGuisrc/crypto/skein.c
  PKGuisrc/crypto/slow-hash.c
  PKGuisrc/crypto/slow-hash.cpp
  PKGuisrc/crypto/tree-hash.c
  PKGuisrc/CryptoNoteCore/BlockchainIndices.cpp
  PKGuisrc/CryptoNoteCore/BlockchainMessages.cpp
  PKGuisrc/CryptoNoteCore/BlockIndex.cpp
  PKGuisrc/CryptoNoteCore/CoreConfig.cpp
  PKGuisrc/CryptoNoteCore/CryptoNoteBasic.cpp
  PKGuisrc/CryptoNoteCore/CryptoNoteTools.cpp
  PKGuisrc/CryptoNoteCore/Currency.cpp
  PKGuisrc/CryptoNoteCore/DepositIndex.cpp
  PKGuisrc/CryptoNoteCore/MinerConfig.cpp
  PKGuisrc/CryptoNoteCore/Transaction.cpp
  PKGuisrc/CryptoNoteCore/Account.cpp
  PKGuisrc/CryptoNoteCore/Blockchain.cpp
  PKGuisrc/CryptoNoteCore/Checkpoints.cpp
  PKGuisrc/CryptoNoteCore/CryptoNoteBasicImpl.cpp
  PKGuisrc/CryptoNoteCore/Core.cpp
  PKGuisrc/CryptoNoteCore/CryptoNoteFormatUtils.cpp
  PKGuisrc/CryptoNoteCore/CryptoNoteSerialization.cpp
  PKGuisrc/CryptoNoteCore/Difficulty.cpp
  PKGuisrc/CryptoNoteCore/IBlock.cpp
  PKGuisrc/CryptoNoteCore/Miner.cpp
  PKGuisrc/CryptoNoteCore/TransactionExtra.cpp
  PKGuisrc/CryptoNoteCore/TransactionPool.cpp
  PKGuisrc/CryptoNoteCore/TransactionPrefixImpl.cpp
  PKGuisrc/CryptoNoteCore/TransactionUtils.cpp
  PKGuisrc/InProcessNode/InProcessNode.cpp
  PKGuisrc/InProcessNode/InProcessNodeErrors.cpp
  PKGuisrc/NodeRpcProxy/NodeErrors.cpp
  PKGuisrc/NodeRpcProxy/NodeRpcProxy.cpp
  PKGuisrc/P2p/NetNodeConfig.cpp
  PKGuisrc/Serialization/BinaryInputStreamSerializer.cpp
  PKGuisrc/Serialization/BinaryOutputStreamSerializer.cpp
  PKGuisrc/Serialization/JsonInputValueSerializer.cpp
  PKGuisrc/Serialization/JsonOutputStreamSerializer.cpp
  PKGuisrc/Serialization/KVBinaryInputStreamSerializer.cpp
  PKGuisrc/Serialization/KVBinaryOutputStreamSerializer.cpp
  PKGuisrc/Serialization/SerializationOverloads.cpp
  PKGuisrc/Transfers/BlockchainSynchronizer.cpp
  PKGuisrc/Transfers/SynchronizationState.cpp
  PKGuisrc/Transfers/TransfersConsumer.cpp
  PKGuisrc/Transfers/TransfersContainer.cpp
  PKGuisrc/Transfers/TransfersSubscription.cpp
  PKGuisrc/Transfers/TransfersSynchronizer.cpp
  PKGuisrc/Wallet/LegacyKeysImporter.cpp
  PKGuisrc/Wallet/WalletAsyncContextCounter.cpp
  PKGuisrc/Wallet/WalletErrors.cpp
  PKGuisrc/WalletLegacy/KeysStorage.cpp
  PKGuisrc/WalletLegacy/WalletLegacy.cpp
  PKGuisrc/WalletLegacy/WalletHelper.cpp
  PKGuisrc/WalletLegacy/WalletLegacySerializer.cpp
  PKGuisrc/WalletLegacy/WalletLegacySerialization.cpp
  PKGuisrc/WalletLegacy/WalletTransactionSender.cpp
  PKGuisrc/WalletLegacy/WalletUnconfirmedTransactions.cpp
  PKGuisrc/WalletLegacy/WalletUserTransactionsCache.cpp
  PKGuisrc/System/ContextGroup.cpp
  PKGuisrc/System/Event.cpp
  PKGuisrc/System/EventLock.cpp
  PKGuisrc/System/InterruptedException.cpp
  PKGuisrc/System/Ipv4Address.cpp
  PKGuisrc/System/TcpStream.cpp
  PKGuisrc/HTTP/HttpRequest.cpp
  PKGuisrc/HTTP/HttpParser.cpp
  PKGuisrc/HTTP/HttpParserErrorCodes.cpp
  PKGuisrc/HTTP/HttpResponse.cpp
  PKGuisrc/Rpc/HttpClient.cpp
  PKGuisrc/Rpc/JsonRpc.cpp
  PKGuisrc/P2p/NetNode.cpp
  PKGuisrc/P2p/LevinProtocol.cpp
  PKGuisrc/P2p/NetNodeConfig.cpp
  PKGuisrc/P2p/PeerListManager.cpp
  PKGuisrc/CryptoNoteProtocol/CryptoNoteProtocolHandler.cpp
  PKGuisrc/Logging/ILogger.cpp
  PKGuisrc/Logging/LoggerGroup.cpp
  PKGuisrc/Logging/CommonLogger.cpp
  PKGuisrc/Logging/LoggerManager.cpp
  PKGuisrc/Logging/FileLogger.cpp
  PKGuisrc/Logging/StreamLogger.cpp
  PKGuisrc/Logging/ConsoleLogger.cpp
  PKGuisrc/Logging/LoggerMessage.cpp
  PKGuisrc/Logging/LoggerRef.cpp
  PKGuisrc/BlockchainExplorer/BlockchainExplorer.cpp
  PKGuisrc/BlockchainExplorer/BlockchainExplorerDataBuilder.cpp
  PKGuisrc/BlockchainExplorer/BlockchainExplorerErrors.cpp
)

file(GLOB_RECURSE SOURCES src/*.cpp)
file(GLOB_RECURSE HEADERS src/*.h)
file(GLOB_RECURSE FORMS src/gui/ui/*.ui)

set(QRC src/resources.qrc)

qt5_wrap_ui(UIS ${FORMS})
qt5_add_resources(RCC ${QRC})


if (WIN32)
  if (NOT MSVC)
	message(FATAL_ERROR "Only MSVC is supported on this platform")
  endif ()
  add_definitions(/D_CRT_SECURE_NO_WARNINGS /D_WIN32_WINNT=0x0600 /DSTATICLIB)
  include_directories(PKGuisrc/platform/msc)

  set(PLATFORM_DIR Windows)
  set(BUILD_PLATFORM WIN32)
  set(BUILD_RESOURCES src/cryptonotewallet.rc)
  set(QTMAIN Qt5::WinMain)

elseif (UNIX)
  set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} PKGuiexternal/miniupnpc/minissdpc.c)
  if (APPLE)
    enable_language(ASM)
    file(GLOB_RECURSE OBJC_SOURCES src/*.mm)
    set(SOURCES ${SOURCES} ${OBJC_SOURCES})
    set(PLATFORM_DIR OSX)
    set(MACOSX_BUNDLE_INFO_STRING "Cryptonote GUI wallet")
    set(MACOSX_BUNDLE_LONG_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_NAME CryptonoteWallet)
    set(MACOSX_BUNDLE_SHORT_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_VERSION "$${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")

    find_package(Qt5PrintSupport REQUIRED)

    include_directories(/usr/include/malloc)

    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -std=c++11 -stdlib=libc++")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c11 -maes -D_DARWIN_C_SOURCE")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -framework Cocoa -framework OpenGL -framework CoreFoundation -framework Carbon -framework IOKit -L/usr/lib")

    set(MACOSX_BUNDLE_ICON_FILE cryptonote.icns)
    set(APPLICATION_ICON src/images/cryptonote.icns)
    set_source_files_properties(${APPLICATION_ICON} PROPERTIES MACOSX_PACKAGE_LOCATION "Resources")

    set(BUILD_PLATFORM MACOSX_BUNDLE)
    set(BUILD_RESOURCES ${APPLICATION_ICON})
	
    GET_TARGET_PROPERTY(QT_LIB_DIR "${Qt5Widgets_LIBRARIES}" LOCATION)
    GET_FILENAME_COMPONENT(QT_LIB_DIR "${QT_LIB_DIR}" PATH)
  else(APPLE)
    set(PLATFORM_DIR Linux)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -std=c++11")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -maes -std=c11")
  endif (APPLE)
endif ()

include_directories(PKGuisrc/Platform/${PLATFORM_DIR})
file(GLOB PLATFORM_SOURCES PKGuisrc/Platform/${PLATFORM_DIR}/System/*)
set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} ${PLATFORM_SOURCES})

add_library(${CRYPTONOTE_LIB} STATIC ${CRYPTONOTE_SOURCES})
set_target_properties(${CRYPTONOTE_LIB} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
target_link_libraries(${CRYPTONOTE_LIB} ${Boost_LIBRARIES})

add_executable(${PROJECT_NAME} ${BUILD_PLATFORM} ${BUILD_RESOURCES} ${SOURCES} ${HEADERS} ${UIS} ${RCC})
set_target_properties(${PROJECT_NAME} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
target_link_libraries(${PROJECT_NAME} ${Boost_LIBRARIES} ${QTMAIN} ${CRYPTONOTE_LIB})

if (APPLE)
  qt5_use_modules(${PROJECT_NAME} PrintSupport)
elseif (UNIX)
  target_link_libraries(${PROJECT_NAME} -lpthread)
elseif (WIN32)
  target_link_libraries(${PROJECT_NAME} Imm32 Iphlpapi Winmm)
endif (APPLE)

qt5_use_modules(${PROJECT_NAME} Widgets Gui Network)

# Installation

set(CPACK_PACKAGE_NAME ${CN_PROJECT_NAME})
set(CPACK_PACKAGE_VERSION ${CN_VERSION})
set(CPACK_PACKAGE_VENDOR "Pennykoin")
set(CPACK_PACKAGE_CONTACT "https://pennykoin.com/")
set(CPACK_STRIP_FILES ON)
if (APPLE)
    set(CPACK_GENERATOR DragNDrop)
    install(TARGETS ${PROJECT_NAME} BUNDLE DESTINATION .)
    set(CPACK_PACKAGE_FILE_NAME "${CPACK_PACKAGE_NAME}-${CPACK_PACKAGE_VERSION}.-${VERSION_RELEASE}")
elseif (UNIX)
    install(TARGETS ${PROJECT_NAME} RUNTIME DESTINATION bin)
    install(FILES src/QTpenny.desktop DESTINATION share/applications)
    install(FILES src/images/cryptonote.png DESTINATION share/pixmaps RENAME pennykoin.png)


	SET(CPACK_PACKAGE_DESCRIPTION_FILE "${CMAKE_CURRENT_SOURCE_DIR}/README.md")
	SET(CPACK_PACKAGE_ICON src/images/cryptonote.png)
	
    if (NOT PACKRPM)
        set(CPACK_GENERATOR DEB)
        set(CPACK_DEBIAN_PACKAGE_NAME ${CPACK_PACKAGE_NAME})
        set(CPACK_SYSTEM_NAME amd64)
        set(CPACK_DEBIAN_PACKAGE_MAINTAINER "${CPACK_PACKAGE_VENDOR} <xrydev@gmail.com>")
        set(CPACK_DEBIAN_PACKAGE_SECTION Office)
        set(CPACK_DEBIAN_PACKAGE_SHLIBDEPS ON)
        set(CPACK_DEBIAN_PACKAGE_DESCRIPTION "QTPenny is the GUI wallet for pennykoin cryptocurrency.")
set(CPACK_PACKAGE_FILE_NAME "${CPACK_DEBIAN_PACKAGE_NAME}-${CPACK_PACKAGE_VERSION}-${VERSION_RELEASE}.${CPACK_SYSTEM_NAME}")
	else ()
        set(CPACK_GENERATOR RPM)
        set(CPACK_SYSTEM_NAME x86_64)
        set(CPACK_RPM_PACKAGE_RELEASE ${VERSION_RELEASE})
        set(CPACK_RPM_PACKAGE_LICENSE "GPL3")
        set(CPACK_RPM_PACKAGE_GROUP Office)
        set(CPACK_RPM_PACKAGE_REQUIRES "qt5-qtbase >= 5.3.2, qt5-qtbase-gui >= 5.3.2")
        set(CPACK_RPM_PACKAGE_SUMMARY "Pennykoin GUI wallet")
        set(CPACK_RPM_PACKAGE_DESCRIPTION "QTPenny is the GUI wallet for pennykoin cryptocurrency.")
set(CPACK_PACKAGE_FILE_NAME "${CPACK_PACKAGE_NAME}-${CPACK_PACKAGE_VERSION}-${VERSION_RELEASE}.${CPACK_SYSTEM_NAME}")
    endif ()
elseif (WIN32)
  install(TARGETS ${PROJECT_NAME} RUNTIME DESTINATION .)
  set(CPACK_GENERATOR ZIP)
endif (APPLE)

include(CPack)
