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
    PKCli/external
    PKCli/include
    PKCli/src)

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
configure_file("PKCli/src/version.h.in" "version.h")
configure_file("src/CryptoNoteWalletConfig.h.in" "CryptoNoteWalletConfig.h")

add_definitions(-DVERSION=\"${CN_VERSION}\")
add_definitions(-DGIT_REVISION=\"${GIT_REVISION}\")

set(CMAKE_AUTOMOC ON)

set(CRYPTONOTE_SOURCES
  PKCli/external/miniupnpc/connecthostport.c
  PKCli/external/miniupnpc/igd_desc_parse.c
  PKCli/external/miniupnpc/minisoap.c
  PKCli/external/miniupnpc/miniupnpc.c
  PKCli/external/miniupnpc/miniwget.c
  PKCli/external/miniupnpc/minixml.c
  PKCli/external/miniupnpc/portlistingparse.c
  PKCli/external/miniupnpc/receivedata.c
  PKCli/external/miniupnpc/upnpcommands.c
  PKCli/external/miniupnpc/upnpreplyparse.c
  PKCli/src/Common/Base58.cpp
  PKCli/src/Common/CommandLine.cpp
  PKCli/src/Common/Util.cpp
  PKCli/src/Common/StringTools.cpp
  PKCli/src/Common/JsonValue.cpp
  PKCli/src/Common/ConsoleTools.cpp
  PKCli/src/Common/MemoryInputStream.cpp
  PKCli/src/Common/PathTools.cpp
  PKCli/src/Common/StdInputStream.cpp
  PKCli/src/Common/StdOutputStream.cpp
  PKCli/src/Common/StreamTools.cpp
  PKCli/src/Common/StringOutputStream.cpp
  PKCli/src/Common/StringView.cpp
  PKCli/src/Common/VectorOutputStream.cpp
  PKCli/src/crypto/blake256.c
  PKCli/src/crypto/chacha.c
  PKCli/src/crypto/crypto-ops-data.c
  PKCli/src/crypto/crypto-ops.c
  PKCli/src/crypto/crypto.cpp
  PKCli/src/crypto/groestl.c
  PKCli/src/crypto/hash-extra-blake.c
  PKCli/src/crypto/hash-extra-groestl.c
  PKCli/src/crypto/hash-extra-jh.c
  PKCli/src/crypto/hash-extra-skein.c
  PKCli/src/crypto/hash.c
  PKCli/src/crypto/jh.c
  PKCli/src/crypto/keccak.c
  PKCli/src/crypto/oaes_lib.c
  PKCli/src/crypto/random.c
  PKCli/src/crypto/skein.c
  PKCli/src/crypto/slow-hash.c
  PKCli/src/crypto/slow-hash.cpp
  PKCli/src/crypto/tree-hash.c
  PKCli/src/CryptoNoteCore/BlockchainIndices.cpp
  PKCli/src/CryptoNoteCore/BlockchainMessages.cpp
  PKCli/src/CryptoNoteCore/BlockIndex.cpp
  PKCli/src/CryptoNoteCore/CoreConfig.cpp
  PKCli/src/CryptoNoteCore/CryptoNoteBasic.cpp
  PKCli/src/CryptoNoteCore/CryptoNoteTools.cpp
  PKCli/src/CryptoNoteCore/Currency.cpp
  PKCli/src/CryptoNoteCore/DepositIndex.cpp
  PKCli/src/CryptoNoteCore/MinerConfig.cpp
  PKCli/src/CryptoNoteCore/Transaction.cpp
  PKCli/src/CryptoNoteCore/Account.cpp
  PKCli/src/CryptoNoteCore/Blockchain.cpp
  PKCli/src/CryptoNoteCore/Checkpoints.cpp
  PKCli/src/CryptoNoteCore/CryptoNoteBasicImpl.cpp
  PKCli/src/CryptoNoteCore/Core.cpp
  PKCli/src/CryptoNoteCore/CryptoNoteFormatUtils.cpp
  PKCli/src/CryptoNoteCore/CryptoNoteSerialization.cpp
  PKCli/src/CryptoNoteCore/Difficulty.cpp
  PKCli/src/CryptoNoteCore/IBlock.cpp
  PKCli/src/CryptoNoteCore/Miner.cpp
  PKCli/src/CryptoNoteCore/TransactionExtra.cpp
  PKCli/src/CryptoNoteCore/TransactionPool.cpp
  PKCli/src/CryptoNoteCore/TransactionPrefixImpl.cpp
  PKCli/src/CryptoNoteCore/TransactionUtils.cpp
  PKCli/src/InProcessNode/InProcessNode.cpp
  PKCli/src/InProcessNode/InProcessNodeErrors.cpp
  PKCli/src/NodeRpcProxy/NodeErrors.cpp
  PKCli/src/NodeRpcProxy/NodeRpcProxy.cpp
  PKCli/src/P2p/NetNodeConfig.cpp
  PKCli/src/Serialization/BinaryInputStreamSerializer.cpp
  PKCli/src/Serialization/BinaryOutputStreamSerializer.cpp
  PKCli/src/Serialization/JsonInputValueSerializer.cpp
  PKCli/src/Serialization/JsonOutputStreamSerializer.cpp
  PKCli/src/Serialization/KVBinaryInputStreamSerializer.cpp
  PKCli/src/Serialization/KVBinaryOutputStreamSerializer.cpp
  PKCli/src/Serialization/SerializationOverloads.cpp
  PKCli/src/Transfers/BlockchainSynchronizer.cpp
  PKCli/src/Transfers/SynchronizationState.cpp
  PKCli/src/Transfers/TransfersConsumer.cpp
  PKCli/src/Transfers/TransfersContainer.cpp
  PKCli/src/Transfers/TransfersSubscription.cpp
  PKCli/src/Transfers/TransfersSynchronizer.cpp
  PKCli/src/Wallet/LegacyKeysImporter.cpp
  PKCli/src/Wallet/WalletAsyncContextCounter.cpp
  PKCli/src/Wallet/WalletErrors.cpp
  PKCli/src/WalletLegacy/KeysStorage.cpp
  PKCli/src/WalletLegacy/WalletLegacy.cpp
  PKCli/src/WalletLegacy/WalletHelper.cpp
  PKCli/src/WalletLegacy/WalletLegacySerializer.cpp
  PKCli/src/WalletLegacy/WalletLegacySerialization.cpp
  PKCli/src/WalletLegacy/WalletTransactionSender.cpp
  PKCli/src/WalletLegacy/WalletUnconfirmedTransactions.cpp
  PKCli/src/WalletLegacy/WalletUserTransactionsCache.cpp
  PKCli/src/System/ContextGroup.cpp
  PKCli/src/System/Event.cpp
  PKCli/src/System/EventLock.cpp
  PKCli/src/System/InterruptedException.cpp
  PKCli/src/System/Ipv4Address.cpp
  PKCli/src/System/TcpStream.cpp
  PKCli/src/HTTP/HttpRequest.cpp
  PKCli/src/HTTP/HttpParser.cpp
  PKCli/src/HTTP/HttpParserErrorCodes.cpp
  PKCli/src/HTTP/HttpResponse.cpp
  PKCli/src/Rpc/HttpClient.cpp
  PKCli/src/Rpc/JsonRpc.cpp
  PKCli/src/P2p/NetNode.cpp
  PKCli/src/P2p/LevinProtocol.cpp
  PKCli/src/P2p/NetNodeConfig.cpp
  PKCli/src/P2p/PeerListManager.cpp
  PKCli/src/CryptoNoteProtocol/CryptoNoteProtocolHandler.cpp
  PKCli/src/Logging/ILogger.cpp
  PKCli/src/Logging/LoggerGroup.cpp
  PKCli/src/Logging/CommonLogger.cpp
  PKCli/src/Logging/LoggerManager.cpp
  PKCli/src/Logging/FileLogger.cpp
  PKCli/src/Logging/StreamLogger.cpp
  PKCli/src/Logging/ConsoleLogger.cpp
  PKCli/src/Logging/LoggerMessage.cpp
  PKCli/src/Logging/LoggerRef.cpp
  PKCli/src/BlockchainExplorer/BlockchainExplorer.cpp
  PKCli/src/BlockchainExplorer/BlockchainExplorerDataBuilder.cpp
  PKCli/src/BlockchainExplorer/BlockchainExplorerErrors.cpp
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
  include_directories(PKCli/src/platform/msc)

  set(PLATFORM_DIR Windows)
  set(BUILD_PLATFORM WIN32)
  set(BUILD_RESOURCES src/cryptonotewallet.rc)
  set(QTMAIN Qt5::WinMain)

elseif (UNIX)
  set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} PKCli/external/miniupnpc/minissdpc.c)
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

include_directories(PKCli/src/Platform/${PLATFORM_DIR})
file(GLOB PLATFORM_SOURCES PKCli/src/Platform/${PLATFORM_DIR}/System/*)
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
