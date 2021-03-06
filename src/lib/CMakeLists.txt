ecm_create_qm_loader(syntax_highlighting_QM_LOADER syntaxhighlighting5_qt)

set(syntax_highlighting_srcs
    abstracthighlighter.cpp
    context.cpp
    contextswitch.cpp
    definitiondownloader.cpp
    foldingregion.cpp
    format.cpp
    htmlhighlighter.cpp
    keywordlist.cpp
    matchresult.cpp
    rule.cpp
    definition.cpp
    repository.cpp
    state.cpp
    syntaxhighlighter.cpp
    theme.cpp
    wildcardmatcher.cpp
    themedata.cpp
    ${syntax_highlighting_QM_LOADER}
)
ecm_qt_declare_logging_category(syntax_highlighting_srcs
    HEADER ksyntaxhighlighting_logging.h
    IDENTIFIER KSyntaxHighlighting::Log
    CATEGORY_NAME org.kde.ksyntaxhighlighting
)

qt5_add_resources(themes_QRC ${CMAKE_SOURCE_DIR}/data/themes/theme-data.qrc)

add_library(KF5SyntaxHighlighting SHARED ${syntax_highlighting_srcs} ${themes_QRC})
generate_export_header(KF5SyntaxHighlighting BASE_NAME KSyntaxHighlighting)
set_target_properties(KF5SyntaxHighlighting PROPERTIES
    VERSION ${SyntaxHighlighting_VERSION_STRING}
    SOVERSION ${SyntaxHighlighting_SOVERSION}
    EXPORT_NAME SyntaxHighlighting
)
target_include_directories(KF5SyntaxHighlighting INTERFACE "$<INSTALL_INTERFACE:${KDE_INSTALL_INCLUDEDIR_KF5}/KSyntaxHighlighting;${KDE_INSTALL_INCLUDEDIR_KF5}>")
target_include_directories(KF5SyntaxHighlighting PUBLIC "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR};${CMAKE_CURRENT_BINARY_DIR};>")
target_link_libraries(KF5SyntaxHighlighting LINK_PUBLIC Qt5::Gui LINK_PRIVATE Qt5::Network SyntaxHighlightingData)

ecm_generate_headers(SyntaxHighlighting_HEADERS
    HEADER_NAMES
        AbstractHighlighter
        Definition
        FoldingRegion
        Format
        Repository
        State
        SyntaxHighlighter
        Theme
    REQUIRED_HEADERS SyntaxHighlighting_HEADERS
)

install(TARGETS KF5SyntaxHighlighting EXPORT KF5SyntaxHighlightingTargets ${INSTALL_TARGETS_DEFAULT_ARGS})
install(FILES
    ${SyntaxHighlighting_HEADERS}
    ${CMAKE_CURRENT_BINARY_DIR}/ksyntaxhighlighting_export.h
    DESTINATION ${KDE_INSTALL_INCLUDEDIR_KF5}/KSyntaxHighlighting)

if(BUILD_QCH)
    ecm_add_qch(
        KF5SyntaxHighlighting_QCH
        NAME KSyntaxHighlighting
        BASE_NAME KF5SyntaxHighlighting
        VERSION ${KF5_VERSION}
        ORG_DOMAIN org.kde
        SOURCES # using only public headers, to cover only public API
            ${SyntaxHighlighting_HEADERS}
        MD_MAINPAGE "${CMAKE_SOURCE_DIR}/README.md"
        LINK_QCHS
            Qt5Core_QCH
            Qt5Gui_QCH
        BLANK_MACROS
            KSYNTAXHIGHLIGHTING_EXPORT
            KSYNTAXHIGHLIGHTING_DEPRECATED
        TAGFILE_INSTALL_DESTINATION ${KDE_INSTALL_QTQCHDIR}
        QCH_INSTALL_DESTINATION ${KDE_INSTALL_QTQCHDIR}
        COMPONENT Devel
    )
endif()
ecm_generate_pri_file(
    BASE_NAME KSyntaxHighlighting LIB_NAME
    KF5SyntaxHighlighting
    DEPS "gui"
    FILENAME_VAR PRI_FILENAME
    INCLUDE_INSTALL_DIR ${KDE_INSTALL_INCLUDEDIR_KF5}/KSyntaxHighlighting
)
install(FILES ${PRI_FILENAME} DESTINATION ${ECM_MKSPECS_INSTALL_DIR})
